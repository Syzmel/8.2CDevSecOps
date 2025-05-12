pipeline {
    agent any
        environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
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
                // Use the injected environment variable (%SONAR_TOKEN%) in your script.
                //if %ERRORLEVEL% NEQ 0 exit /b 0
                //-Dsonar.organization=syzmel ^
                bat '''
                  sonar-scanner ^
                  -Dsonar.projectKey=sit223 ^
                  
                  -Dsonar.sources=. ^
                  -Dsonar.host.url=https://sonarcloud.io ^
                  -Dsonar.login=ae3e0cd85e60d4e43416a9ebf03d827702acd046
                  
                '''
            }
        }
    }
}
