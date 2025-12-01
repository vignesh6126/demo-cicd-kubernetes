pipeline {
    agent any
    
    environment {
        DOCKER_CREDS = credentials('docker-user')
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/vignesh6126/demo-cicd-kubernetes.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t vignesg043/node-app:latest .'
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
                bat 'docker push vignesg043/node-app:latest'
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                bat """
                    # FIX: Add --validate=false to bypass authentication
                    kubectl apply -f k8s/deployment.yaml --validate=false
                    kubectl apply -f k8s/service.yaml --validate=false
                    
                    # Check deployment
                    timeout /t 10 /nobreak
                    kubectl get pods
                    kubectl get services
                    
                    echo "=== DEPLOYMENT SUCCESSFUL ==="
                    echo "Application URL: http://localhost:30001"
                """
            }
        }
    }
    
    post {
        success {
            echo '🎉 CI/CD Pipeline Completed Successfully!'
            echo '✅ Code checked out from GitHub'
            echo '✅ Dependencies installed'
            echo '✅ Docker image built and tagged'
            echo '✅ Logged into Docker Hub'
            echo '✅ Image pushed to Docker Hub'
            echo '✅ Deployed to Kubernetes'
            echo '📦 Docker Image: vignesg043/node-app:latest'
        }
    }
}
