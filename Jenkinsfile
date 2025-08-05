pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-node-app"
        DOCKER_HUB_USER = "your-dockerhub-username" // replace this
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
                bat 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest
                    """
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying Docker container...'
                bat 'docker rm -f my-running-app || true'
                bat 'docker run -d -p 3000:3000 --name my-running-app $DOCKER_HUB_USER/$IMAGE_NAME:latest'
            }
        }
    }
}
