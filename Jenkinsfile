pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/kamu/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh 'trivy image my-app:latest || true'
            }
        }

        stage('Push to Minikube') {
            steps {
                sh 'eval $(minikube docker-env)'
                sh 'docker build -t my-app:latest .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
