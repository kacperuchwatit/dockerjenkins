pipeline {
  agent any
  
  triggers {
    githubPush()
  }
  
  environment {
    DOCKER_REGISTRY = "943696080604.dkr.ecr.us-west-1.amazonaws.com"
    ECR_REPOSITORY = "dockerjenkins"
    IMAGE_TAG = "${env.BUILD_NUMBER}"
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
          docker.withRegistry("https://${DOCKER_REGISTRY}", 'aws-credentials') {
            def image = docker.build("${DOCKER_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}", '.')
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
