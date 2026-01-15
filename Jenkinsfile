pipeline {
    agent any

    environment {
        // 
        PATH = "${tool name: 'NodeJS', type: 'NodeJS'}/bin;${env.PATH}"
        // Vendos kubeconfig për Minikube
        KUBECONFIG = "C:\\Users\\MD\\.kube\\config"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/mergimdaki31-debug/node-ci-cd-app.git']]
                ])
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
                bat 'docker build -t daki25/node-ci-cd-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push daki25/node-ci-cd-app:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Deploy me kubeconfig të Minikube
                bat 'kubectl apply -f k8s-deployment.yaml --validate=false'
                bat 'kubectl apply -f k8s-service.yaml --validate=false'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
