pipeline {
    agent any

    environment {
        IMAGE_NAME = "ravipal33/bootstrap"
        IMAGE_TAG = "1.0"
        DOCKER_HUB_CREDENTIALS = "docker-hub-cred"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/ravi33pal-cyber/Bootstrap.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_HUB_CREDENTIALS}",
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    bat "docker login -u %USERNAME% -p %PASSWORD%"
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f k8s-deployment.yaml"
                bat "kubectl apply -f k8s-service.yaml"
            }
        }
    }
}
