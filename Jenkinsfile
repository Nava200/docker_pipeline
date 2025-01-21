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
                // Sending a success notification email
                emailext(
                    subject: "Build Success: Docker image pushed successfully",
                    body: """
                    Build succeeded! 🎉

                    Docker image: ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} has been pushed successfully.

                    Best regards,
                    Jenkins CI/CD
                    """,
                    to: 'nava.priya2000@gmail.com'  // Replace with your email address
                )
            }
        }

        failure {
            script {
                // Sending a failure notification email
                emailext(
                    subject: "Build Failed: Docker image push failed",
                    body: """
                    Build failed! 🚨

                    Docker image: ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} push failed.

                    Please check the Jenkins job logs for more details.

                    Best regards,
                    Jenkins CI/CD
                    """,
                    to: 'nava.priya2000@gmail.com'  // Replace with your email address
                )
            }
        }
    }
}
