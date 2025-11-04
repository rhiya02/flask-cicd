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

                stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'   // Path set in Jenkins Global Tool Configuration
                    withSonarQubeEnv('MySonarQube') {        // "MySonarQube" must match your SonarQube server name in Jenkins
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=flask-cicd \
                            -Dsonar.sources=. \
                            -Dsonar.exclusions=**/tests/**,**/static/**,**/templates/**,**/__pycache__/**,**/node_modules/** \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sqa_80bc4d45f22a55017ee31aa507c34f52e52f72ff
                        """
                    }
                }
            }
        }
    



        stage('Build Docker Image') {
            steps {
                script {
                    echo "🏗️ Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    echo "🧹 Cleaning up old container (if any)..."
                    sh "docker rm -f ${CONTAINER_NAME} || true"    // Cleanup before running

                    echo "🚀 Running new container..."
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}"

                    echo "✅ Container started successfully!"
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
