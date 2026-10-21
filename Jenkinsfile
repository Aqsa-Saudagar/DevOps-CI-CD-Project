
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
    }
}

