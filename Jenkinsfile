pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t daki25/node-ci-cd-app ."
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                echo %DOCKERHUB_CREDENTIALS_PSW% | docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin
                docker push daki25/node-ci-cd-app
                """
            }
        }
    }
}
