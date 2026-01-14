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
                    credentialsId: 'dockerhub-nodejs' //
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
        withCredentials([string(credentialsId: 'dockerhub-nodejs', variable: 'DOCKER_PASS')]) {
            bat 'echo %DOCKER_PASS% | docker login -u daki25 --password-stdin'
            bat 'docker push dagi25/nodejs-jenkins:latest'
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
