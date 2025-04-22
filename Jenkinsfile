pipeline {
  agent any

  environment {
    IMAGE_NAME = "simple-devsecops:latest"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/dedragz17/simple-devsecops.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Security Scan with Trivy') {
      steps {
        script {
            if (!fileExists('/usr/local/bin/trivy')) {
                echo 'Trivy not found, skipping security scan.'
            } else {
                echo 'Running security scan with Trivy...'
                sh 'trivy image --severity HIGH,CRITICAL $IMAGE_NAME || true'
            }
        }
      }
    }

    stage('Deploy to Minikube') {
      steps {
        script {
            echo 'This part we ran manually'
        }
      }
    }
  }
}
