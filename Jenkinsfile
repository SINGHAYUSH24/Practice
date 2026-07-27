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
        
        stage('Deploy to kubernetes') {
            steps {
                sh '''
                KUBECTL="/mnt/c/Program Files/Docker/Docker/resources/bin/kubectl.exe"

            "$KUBECTL" config current-context
            "$KUBECTL" get nodes
            "$KUBECTL" get pods -n nginxapp

            "$KUBECTL" set image deployment/web-app-deployment \
                web-app=singhayush24/webapp:2 \
                -n nginxapp

            "$KUBECTL" rollout status deployment/web-app-deployment \
                -n nginxapp
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