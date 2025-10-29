pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-website-builder'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourname/website-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", ".")
                }
            }
        }

        stage('Build Website') {
            agent {
                docker {
                    image "${IMAGE_NAME}"
                    reuseNode true
                }
            }
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            agent {
                docker {
                    image "${IMAGE_NAME}"
                    reuseNode true
                }
            }
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
