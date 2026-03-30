pipeline {
    agent any
    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "<AWS_ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/flask-devops"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/flask-devops-eks.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-devops .'
            }
        }
        stage('Push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO
                docker tag flask-devops:latest $ECR_REPO:latest
                docker push $ECR_REPO:latest
                '''
            }
        }
        stage('Deploy to EKS (elastic kubernetes service') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
