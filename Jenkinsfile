pipeline {
    agent any
    environment {
        IMAGE_NAME = "mahender397/jenkins-docker-demo"
        IMAGE_TAG = "latest"
        DOCKER_CREDS = "dockerhub-creds"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mahi3297/docker-jenkins-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }
    post {
        always {
            sh "docker logout"
        }
        success {
            echo "Docker image pushed successfully to Docker Hub!"
        }
    }
}

