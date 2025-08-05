pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'yourdockerhubusername'
        IMAGE_NAME = 'node-docker-app'
    }
    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm packages...'
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat "docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:latest ."
            }
        }
        stage('Push to Docker Hub') {
    steps {
        echo 'Pushing Docker image to Docker Hub...'
        withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDS', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
            bat "docker push %DOCKER_USER%/%IMAGE_NAME%:latest"
        }
    }
}

        }
        stage('Deploy Container') {
            steps {
                echo 'Deploying Docker container...'
                bat "docker run -d -p 3000:3000 %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
            }
        }
    }
}
