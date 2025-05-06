pipeline {
    agent any

    environment {
        IMAGE_NAME = 'your-dockerhub-username/flask-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = 'dockerhub-creds' // Configure in Jenkins Credentials
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/your-org/your-flask-repo.git'
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
                echo "Deploying ${IMAGE_NAME}:${IMAGE_TAG}..."
                // Add kubectl/SSH deployment commands here
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
