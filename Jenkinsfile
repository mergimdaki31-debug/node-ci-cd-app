pipeline {
    agent any

    environment {
        IMAGE_NAME = "dagi25/nodejs-jenkins:latest" // ose mergimdaki31/nodejs-jenkins:latest nëse repo ekziston
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git(
                    url: 'https://github.com/mergimdaki31-debug/node-ci-cd-app.git',
                    branch: 'main',
                    credentialsId: 'dockerhub-nodejs' // credential për Git, nëse e ke
                )
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
                bat "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-token', // credential me username + PAT që krijove
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                    bat "docker push ${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout' // opsionale, për siguri
        }
        success {
            echo 'Pipeline run successfully and image pushed!'
        }
        failure {
            echo 'Pipeline failed! Check logs above.'
        }
    }
}
