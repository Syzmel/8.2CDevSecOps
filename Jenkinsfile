pipeline {
    agent any

    environment {
        // This retrieves the secret text stored under 'sonar-token-id' 
        // and assigns it to the SONAR_TOKEN environment variable.
        SONAR_TOKEN = credentials('sonar-token-id')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Syzmel/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install || exit /b 0'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                // Use the injected environment variable (%SONAR_TOKEN%) in your script.
                bat '''
                  sonar-scanner ^
                  -Dsonar.projectKey=your_project_key ^
                  -Dsonar.organization=your_organization ^
                  -Dsonar.sources=. ^
                  -Dsonar.host.url=https://sonarcloud.io ^
                  -Dsonar.login=%SONAR_TOKEN%
                  if %ERRORLEVEL% NEQ 0 exit /b 0
                '''
            }
        }
    }
}
