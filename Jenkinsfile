pipeline {
    agent any
    environment {
        // Include NodeJS in the PATH
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        // Securely inject your SonarCloud token from Jenkins credentials
        SONAR_TOKEN = credentials('ae3e0cd85e60d4e43416a9ebf03d827702acd046')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Syzmel/8.2CDevSecOps.git'
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
        }
        stage('SonarCloud Analysis') {
            steps {
                bat '''
                    sonar-scanner-cli-4.8.0.2856-windows\\bin\\sonar-scanner.bat ^
                    -Dsonar.projectKey=sit223 ^
                    -Dsonar.organization=syzmel ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=https://sonarcloud.io ^
                    -Dsonar.login=%SONAR_TOKEN%
                    if %ERRORLEVEL% NEQ 0 exit /B 0
                '''
            }
        }
    }
}
