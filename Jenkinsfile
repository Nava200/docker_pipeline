pipeline {
    agent {
        node {
            label 'node2'
        }
    }
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds') // Replace with your Jenkins credentials ID
        DOCKER_IMAGE_NAME = 'navaneetha084/tomcat'
        GOOGLE_CHAT_SCRIPT_URL = 'YOUR_GOOGLE_SCRIPT_URL' // Replace with your Google Apps Script URL
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
            script {
                sendGoogleChatNotification("Build succeeded! 🎉 Docker image ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} pushed successfully.")
            }
        }
        failure {
            script {
                sendGoogleChatNotification("Build failed! ❌ Please check the Jenkins logs for details.")
            }
        }
    }
}

def sendGoogleChatNotification(String message) {
    def scriptURL = https://groups.google.com/g/jenkins_pipeline_notification // Use the Google Apps Script URL

    sh """
    curl -X POST -H 'Content-Type: application/json' \
    -d '{"text": "${message}"}' \
    ${scriptURL}
    """
}
