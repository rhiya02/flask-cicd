pipeline {
    agent any

    environment {
        IMAGE_NAME = "rhiya02/flask-cicd"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "flask_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rhiya02/flask-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    echo "Stopping and removing old container if it exists..."
                    sh "docker rm -f ${CONTAINER_NAME} || true"

                    echo "Running new container..."
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}"

                    echo "Container started successfully!"
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build or deployment failed! Showing Docker logs..."
            sh "docker logs ${CONTAINER_NAME} || true"
        }
        success {
            echo "✅ Pipeline completed successfully!"
            sh "docker ps"
        }
    }
}


