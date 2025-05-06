pipeline {
    agent any

    environment {
        IMAGE_NAME = 'sriatmakuri/app.py'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = 'dockerhub-creds' // Configure in Jenkins Credentials
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/sriravi25/app.py'
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
                sh 'pytest tests/' // or your test runner
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
                    docker stop app.py || true
                    docker rm app.py || true
                    docker run -d --name app.py -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}
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
