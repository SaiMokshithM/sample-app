pipeline {
    agent any

    environment {
        IMAGE_NAME = "saimokshith/sample-app"
        TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SaiMokshithM/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%TAG% .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %IMAGE_NAME%:%TAG%'
            }
        }

    }

    post {
        success {
            echo 'Docker Image Pushed Successfully!'
        }

        failure {
            echo 'Pipeline Failed!'
        }

        always {
            bat 'docker logout'
        }
    }
}