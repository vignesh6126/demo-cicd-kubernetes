pipeline {
    agent any
    
    environment {
        DOCKER_CREDS = credentials('docker-user')
        IMAGE_NAME = "vignesg043/node-app"
        IMAGE_TAG = "latest"
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                bat "npm install"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                bat """
                    echo %DOCKER_CREDS_PSW% | docker login -u %DOCKER_CREDS_USR% --password-stdin
                """
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                bat """
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl rollout status deployment/node-app
                """
            }
        }
    }
}
