pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Check Node') {
            steps {
                bat 'node -v'
                bat 'npm -v'
            }
        }

        stage('Install') {
            steps {
                bat 'npm install'
            }
        }
    }

    post {
        success {
            echo 'Pipeline OK'
        }
        failure {
            echo 'Pipeline FAILED'
        }
    }
}
