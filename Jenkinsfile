pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "<your_aws_account_id>"
        AWS_REGION = "ap-south-1"   // change to your AWS region
        ECR_REPO = "nginx-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/<your-username>/nginx-jenkins-ecr-minikube.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                docker build -t $ECR_REPO:$IMAGE_TAG .
                docker tag $ECR_REPO:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                """
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh "docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG"
            }
        }
        stage('Deploy to Minikube') {
            steps {
                sh """
                kubectl apply -f deployment.yaml
                kubectl rollout status deployment/nginx-app
                """
            }
        }
    }
}
