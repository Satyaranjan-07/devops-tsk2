pipeline {
    agent any
    environment {
        DOCKER_USER = 'xdivine07'
        IMAGE_NAME = 'node-docker-app'
    }
    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_USER%/%IMAGE_NAME%:latest ."
            }
        }
        stage('Verify Image') {
            steps {
                bat "docker images"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS')]) {
                    bat "docker login -u %DOCKER_HUB_USER% -p %DOCKER_HUB_PASS%"
                    bat "docker push %DOCKER_USER%/%IMAGE_NAME%:latest"
                }
            }
        }
    }
}
