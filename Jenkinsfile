pipeline {
    agent {
        node {
            label 'node2'
        }
    }
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds') // Replace with your Jenkins credentials ID
        DOCKER_IMAGE_NAME = 'navaneetha084/tomcat'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Nava200/docker.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} ${DOCKER_IMAGE_NAME}:latest
                    """
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    sh """
                    echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin
                    """
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                    docker push ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                    docker push ${DOCKER_IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
    post {
        success {
            echo "Docker image pushed successfully!"
        }
        failure {
            echo "Build failed!"
        }
    }
}



