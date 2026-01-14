pipeline {
    agent any

    environment {
        // Emri i imazhit Docker
        DOCKER_IMAGE = "mergimdaki31/nodejs-jenkins"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Përdor "bat" për Windows
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
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                // Kjo përdor credential ID që ke krijuar në Jenkins
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-nodejs', 
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS')]) {
                    
                    // Login në Docker Hub
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                    
                    // Push image në Docker Hub
                    bat "docker push %DOCKER_IMAGE%:latest"
                }
            }
        }

        // Opsionale: Stage për Kubernetes
        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploy stage këtu nëse ke K8s"
            }
        }
    }
}
