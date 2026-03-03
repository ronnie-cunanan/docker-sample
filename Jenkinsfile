pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                script {
                    // Builds the image from the Dockerfile in the root directory
                    sh "docker build -t docker-sample ."
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    // Stop and remove existing container if it exists
                    sh "docker stop my-running-app || true"
                    sh "docker rm my-running-app || true"
                    // Run the new container
                    sh "docker run -d --name my-running-app -p 8081:5000 docker-sample"
                }
            }
        }
    }
}