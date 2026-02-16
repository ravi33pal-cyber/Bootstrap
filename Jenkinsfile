pipeline {
    agent any

    environment {
        IMAGE_NAME = "ravipal33/bootstrap"
        IMAGE_TAG = "1.0"
        DOCKER_HUB_CREDENTIALS = "dockerhub-creds"
    }

    stages {

        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Docker Login') {
            when {
                branch 'main'
            }
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
            when {
                branch 'main'
            }
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                branch 'main'
            }
            steps {
                bat "kubectl apply -f k8s-deployment.yaml"
                bat "kubectl apply -f k8s-service.yaml"
            }
        }
    }
}
