# Jenkins → Amazon ECR → Minikube

## 🚀 Flow
1. Jenkins builds Docker image from Dockerfile
2. Pushes image to Amazon ECR
3. Deploys/updates app in Minikube

## 🔧 Setup
- Create ECR repo:
  ```bash
  aws ecr create-repository --repository-name nginx-app

