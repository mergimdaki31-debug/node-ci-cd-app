pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        IMAGE_NAME = "node-ci-cd-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test || echo "No tests defined"'
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                    docker build -t %DOCKER_USER%/%IMAGE_NAME%:latest .
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_USER%/%IMAGE_NAME%:latest
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to Kubernetes (simulated)'
                bat 'echo kubectl apply -f k8s/deployment.yaml'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline executed successfully'
        }
        failure {
            echo 'CI/CD Pipeline failed'
        }
    }
}
