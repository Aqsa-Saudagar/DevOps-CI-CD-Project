
pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REGISTRY = '325355907175.dkr.ecr.ap-south-1.amazonaws.com'
        ECR_REPOSITORY = 'devops-web'
        EKS_DEPLOYMENT = 'devops-web'
        EKS_CONTAINER = 'devops-web'
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
        APP_SERVER = 'ubuntu@172.31.8.13'
        SSH_KEY = '/var/lib/jenkins/.ssh/deploy_key'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Aqsa-Saudagar/DevOps-CI-CD-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t devops-web:${BUILD_NUMBER} .
                    docker tag devops-web:${BUILD_NUMBER} devops-web:v1
                '''
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region "$AWS_REGION" |
                    docker login --username AWS --password-stdin "$ECR_REGISTRY"
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh '''
                    docker tag devops-web:${BUILD_NUMBER} \
                    "$ECR_REGISTRY/$ECR_REPOSITORY:${BUILD_NUMBER}"

                    docker push "$ECR_REGISTRY/$ECR_REPOSITORY:${BUILD_NUMBER}"
                '''
            }
        }

        stage('Deploy to Existing EC2') {
            steps {
                sh '''
                    docker save devops-web:v1 |
                    ssh -i "$SSH_KEY" -o IdentitiesOnly=yes \
                    -o StrictHostKeyChecking=yes "$APP_SERVER" \
                    'sudo docker load'

                    ssh -i "$SSH_KEY" -o IdentitiesOnly=yes \
                    -o StrictHostKeyChecking=yes "$APP_SERVER" \
                    'sudo docker rm -f devops-web-container || true; sudo docker run -d --name devops-web-container --restart unless-stopped -p 80:80 devops-web:v1'
                '''
            }
        }

        stage('Deploy to Amazon EKS') {
            steps {
                sh '''
                    kubectl set image deployment/"$EKS_DEPLOYMENT" \
                    "$EKS_CONTAINER"="$ECR_REGISTRY/$ECR_REPOSITORY:${BUILD_NUMBER}"

                    kubectl rollout status deployment/"$EKS_DEPLOYMENT" \
                    --timeout=180s
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Image pushed and deployments updated.'
        }
        failure {
            echo 'FAILED: Check the stage logs to find the issue.'
        }
    }
}
