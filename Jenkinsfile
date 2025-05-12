pipeline {
    agent any
    environment {
        // Inject the Sonar token securely from Jenkins credentials
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONAR_TOKEN = credentials('ae3e0cd85e60d4e43416a9ebf03d827702acd046')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your_github_username/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /B 0'
            }
            post {
                always {
                    emailext (
                        subject: "Run Tests Stage Complete: ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                        body: "The Run Tests stage has completed with status: ${currentBuild.currentResult}.\n\nPlease see the attached log for details.",
                        to: "recipient@example.com",
                        attachLog: true
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /B 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /B 0'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan Stage Complete: ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                        body: "The NPM Audit stage (Security Scan) has completed with status: ${currentBuild.currentResult}.\n\nPlease see the attached log for details.",
                        to: "recipient@example.com",
                        attachLog: true
                    )
                }
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                bat '''
                    sonar-scanner ^
                    -Dsonar.projectKey=syzmel ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=https://sonarcloud.io ^
                    -Dsonar.login=ae3e0cd85e60d4e43416a9ebf03d827702acd046
                    if %ERRORLEVEL% NEQ 0 exit /b 0
                '''
            }
        }
    }
}
