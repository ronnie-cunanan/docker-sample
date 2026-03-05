pipeline {
    agent any

    environment {
        AWS_REGION = "ap-southeast-2"
        AWS_ACCOUNT_ID = "962047682202"
        ECR_REPO = "my-docker-repo"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "my-running-app"
        ECR_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
        
        registry = "962047682202.dkr.ecr.ap-southeast-2.amazonaws.com/my-docker-repo"
    }
    stages {
        stage ('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ronnie-cunanan/docker-sample.git']])
            }
        }
        stage ('Docker Build'){
            steps {
                script {
                    dockerimage = docker.build registry
                }
            }
        }
        stage ('Docker Push') {
            steps {
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                sh "docker push ${ECR_URL}:${IMAGE_TAG}"
            }
        }
        stage ('Deploy/Run container from ECR') {
            steps {
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker run -d --name ${CONTAINER_NAME} -p 8081:5000 ${ECR_URL}:${IMAGE_TAG}"
            }
        }
    }
}