pipeline {
    agent any

    environment {
        IMAGE_NAME = 'sriatmakuri/flask-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = 'dockerhub-creds' // Configure in Jenkins Credentials
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/sriravi25/app.py',
                    branch: 'main'
                
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pip install pytest'
                sh 'pytest tests/'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${REGISTRY_CREDENTIALS}") {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker stop flask-app || true
                    docker rm flask-app || true
                    docker run -d --name flask-app -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
}
