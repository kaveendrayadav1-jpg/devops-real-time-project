pipeline {
  agent any

  environment {
    AWS_REGION = "ap-south-1"
    ECR_URI = "<account-id>.dkr.ecr.ap-south-1.amazonaws.com/myapp"
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/yourname/devops-real-time-project.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t myapp .'
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $AWS_REGION |
        docker login --username AWS --password-stdin $ECR_URI
        docker tag myapp:latest $ECR_URI:latest
        docker push $ECR_URI:latest
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}
