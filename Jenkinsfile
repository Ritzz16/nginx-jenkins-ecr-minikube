pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "720226180820"
        AWS_REGION = "ap-south-1"   // change to your AWS region
        ECR_REPO = "nginx-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Ritzz16/nginx-jenkins-ecr-minikube.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 720226180820.dkr.ecr.ap-south-1.amazonaws.com
                docker build -t nginx-app .
                docker tag nginx-app:latest 720226180820.dkr.ecr.ap-south-1.amazonaws.com/nginx-app:latest
                """
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh "docker push 720226180820.dkr.ecr.ap-south-1.amazonaws.com/nginx-app:latest"
            }
        }
        stage('Deploy to Minikube') {
            steps {
                sh """
                minikube kubectl apply -f deployment.yaml
                minikube kubectl rollout status deployment/nginx-app
                """
            }
        }
    }
}
