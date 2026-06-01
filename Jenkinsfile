pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials')
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout From GitHub') {
            steps {
                git branch: 'main',
                url: 'https://github.com/SINGHAYUSH24/Practice.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t singhayush24/webapp:${IMAGE_TAG} .
                '''
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh '''
                echo "$DOCKER_CREDENTIALS_PSW" | docker login -u "$DOCKER_CREDENTIALS_USR" --password-stdin
                docker push singhayush24/webapp:${IMAGE_TAG}
                '''
            }
        }
        stage('Restart Docker Container') {
            steps {
                sh '''
                docker stop webapp || true
                docker rm webapp || true
                docker run -d --name webapp -p 80:80 singhayush24/webapp:${IMAGE_TAG}
                '''
            }
        }
    }
    post {
        success {
            echo "Pipeline Built Succesfully"
        }

        failure {
            echo "Error in Pipeline"
        }
    }
}