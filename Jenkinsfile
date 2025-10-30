pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/riya/flask-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("riya/flask-cicd:latest")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    dockerImage.run("-d -p 5000:5000")
                }
            }
        }
    }
}
