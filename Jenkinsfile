pipeline {
  agent any

  environment {
    AWS_REGION = "us-east-1"
    ECR_REGISTRY = "218552304265.dkr.ecr.us-east-1.amazonaws.com"
    ECR_REPO = "myapp"
    ECR_URI = "${ECR_REGISTRY}/${ECR_REPO}"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/kaveendrayadav1-jpg/devops-real-time-project.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $ECR_URI:latest .'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION \
          | docker login --username AWS --password-stdin $ECR_REGISTRY
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        sh 'docker push $ECR_URI:latest'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }

  }
}
