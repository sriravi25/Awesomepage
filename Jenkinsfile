pipeline {
    agent any

    environment {
        DEPLOY_DIR = "/var/www/html"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/your-username/your-repo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "Static site — no build step needed."
            }
        }

        stage('Test') {
            steps {
                echo "No automated tests for static page. Skipping..."
                // You can integrate lighthouse or htmlhint for checks
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to $DEPLOY_DIR"
                // Ensure Jenkins has permissions to copy files to this dir
                sh '''
                    sudo rm -rf $DEPLOY_DIR/*
                    sudo cp -r * $DEPLOY_DIR/
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Something went wrong...'
        }
    }
}
