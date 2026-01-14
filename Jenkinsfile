pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mergimdaki31/nodejs-jenkins"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
    }

    stages {

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
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDENTIALS_ID,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                      echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                      docker push %DOCKER_IMAGE%:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}
