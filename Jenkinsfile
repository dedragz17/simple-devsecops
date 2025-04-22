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
        sh '''
          if ! command -v trivy &> /dev/null; then
            echo "Installing Trivy..."
            curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin
          fi
          trivy image --severity HIGH,CRITICAL $IMAGE_NAME || true
        '''
      }
    }

    stage('Deploy to Minikube') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }
}
