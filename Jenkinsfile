pipeline {
	agent none  stages {
  	stage('Maven Install') {
    	agent {
      	docker {
        	image 'maven:3.5.0'
        }
      }
      environment {
        ECR_REGISTRY = "943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins"
        AWS_REGION = "us-west-1"
        DOCKER_IMAGE = "docker_jenkins"
      }
      steps {
      	sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
    	agent any
      steps {
      	sh 'docker build -t shanem/spring-petclinic:latest .'
      }
    }
    stage('Push to ECR') {
            steps {
                script {
                    withCredentials([awsEcr(credentialsId: 'aws-credentials', region: AWS_REGION, registryId: '')]) {
                        docker.withRegistry("https://${ECR_REGISTRY}", 'ecr') {
                            dockerImage.push()
                        }
                    }
                }
            }
    }
  }
}
