pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://192.168.1.21:9000'
        SONARQUBE_TOKEN = 'sqp_aabc6eab666c46768de5498d617944193625b0c8'
    }

    stages {
        stage ('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }

        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONARQUBE_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Check for SonarQube Report') {
            steps {
                script {
                    sh 'ls -al .scannerwork'
                }
            }
        }
    }

    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}
