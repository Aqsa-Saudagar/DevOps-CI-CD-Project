
pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Aqsa-Saudagar/DevOps-CI-CD-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-web:v1 .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'docker images devops-web'
            }
        }

        stage('Transfer Image to App Server') {
            steps {
                sh '''
                    docker save devops-web:v1 | ssh -i /var/lib/jenkins/.ssh/deploy_key -o IdentitiesOnly=yes ubuntu@172.31.8.13 'sudo docker load'
                '''
            }
        }

        stage('Deploy to App Server') {
            steps {
                sh '''
                    ssh -i /var/lib/jenkins/.ssh/deploy_key -o IdentitiesOnly=yes ubuntu@172.31.8.13 'sudo docker rm -f devops-web-container || true; sudo docker run -d --name devops-web-container --restart unless-stopped -p 80:80 devops-web:v1'
                '''
            }
        }
    }
}
