pipeline {
    agent any
    
    environment {
        DOCKER_CREDS = credentials('docker-user')
    }
    
    stages {
        stage('CI: Build and Push') {
            steps {
                bat '''
                    @echo off
                    echo ====================================
                    echo CI/CD PIPELINE - BUILD PHASE
                    echo ====================================
                    
                    echo Step 1: Installing dependencies...
                    npm install
                    
                    echo Step 2: Building Docker image...
                    docker build -t vignesg043/node-app:latest .
                    
                    echo Step 3: Logging into Docker Hub...
                    echo %DOCKER_CREDS_PSW% | docker login -u %DOCKER_CREDS_USR% --password-stdin
                    
                    echo Step 4: Pushing to Docker Hub...
                    docker push vignesg043/node-app:latest
                    
                    echo.
                    echo [SUCCESS] Build phase completed!
                    echo Image: vignesg043/node-app:latest
                    echo.
                '''
            }
        }
        
        stage('CD: Deployment Summary') {
            steps {
                bat '''
                    @echo off
                    echo ====================================
                    echo CI/CD PIPELINE - DEPLOYMENT PHASE
                    echo ====================================
                    
                    echo.
                    echo DEPLOYMENT INSTRUCTIONS:
                    echo 1. Ensure Kubernetes is running in Docker Desktop
                    echo 2. Open terminal and run:
                    echo    kubectl apply -f k8s/deployment.yaml
                    echo    kubectl apply -f k8s/service.yaml
                    echo 3. Verify deployment:
                    echo    kubectl get pods
                    echo    kubectl get services
                    echo 4. Access application:
                    echo    http://localhost:30001
                    echo.
                    echo ====================================
                    echo CI/CD PIPELINE EXECUTED SUCCESSFULLY
                    echo ====================================
                '''
            }
        }
    }
}
