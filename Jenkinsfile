pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "daki25/node-ci-cd-app"
        KUBECONFIG = "C:\\Users\\MD\\.kube\\config" // rregullo path sipas konfigurimit tënd
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mergimdaki31-debug/node-ci-cd-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker login -u daki25 -p YOUR_DOCKERHUB_PASSWORD"
                bat "docker push %DOCKER_IMAGE%"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f k8s-deployment.yaml"
                bat "kubectl apply -f k8s-service.yaml"
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
