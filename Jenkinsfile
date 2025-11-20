pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_URL = "595949345821.dkr.ecr.ap-south-1.amazonaws.com/devops-eks-app"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/devops-eks-project.git'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ECR_URL
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t devops-eks-app:$IMAGE_TAG .
                docker tag devops-eks-app:$IMAGE_TAG $ECR_URL:$IMAGE_TAG
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker push $ECR_URL:$IMAGE_TAG
                '''
            }
        }
    }
}
