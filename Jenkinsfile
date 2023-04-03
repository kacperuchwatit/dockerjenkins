pipeline {
  agent any
  
  triggers {
    githubPush()
  }
  
  environment {
    DOCKER_REGISTRY = "943696080604.dkr.ecr.us-west-1.amazonaws.com"
    ECR_REPOSITORY = "dockerjenkins"
    IMAGE_TAG = "docker_jenkins"
  }
  
  stages {
    stage('Checkout') {
      steps {
        git branch: 'jenkins-ecr', url: 'https://github.com/kacperuchwatit/dockerjenkins'
      }
    }
    
    stage('Build and Push') {
      steps {
        script {
          docker.withRegistry('https://943696080604.dkr.ecr.us-west-1.amazonaws.com', 'ecr:us-west-1:aws-credentials') {
            def image = docker.build("943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:docker_jenkins", '.')
            image.push()
          }
        }
      }
    }
    
    stage('Deploy') {
      when {
        branch 'jenkins-ecr'
      }
      steps {
        sh './deploy.sh'
      }
    }
  }
}
