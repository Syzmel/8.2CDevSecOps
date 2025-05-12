pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Syzmel/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" install'
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    def ret = bat(script: '"C:\\Program Files\\nodejs\\npm.cmd" test', returnStatus: true)
                    echo "npm test exit code: ${ret}"
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                script {
                    def ret = bat(script: '"C:\\Program Files\\nodejs\\npm.cmd" run coverage', returnStatus: true)
                    echo "npm run coverage exit code: ${ret}"
                }
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                script {
                    def ret = bat(script: '"C:\\Program Files\\nodejs\\npm.cmd" audit', returnStatus: true)
                    echo "npm audit exit code: ${ret}"
                }
            }
        }
    }
}
