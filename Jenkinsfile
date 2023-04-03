pipeline {
    agent any

    environment {
        ECR_REGISTRY = "943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins"
        AWS_REGION = "us-west-1"
        DOCKER_IMAGE = "docker_jenkins"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        
        stage('Push to ECR') {
            steps {
                script {
                    withCredentials([awsEcr(credentialsId: 'aws-credentials', region: us-west-1, registryId: '')]) {
                        docker.withRegistry("https://943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins", 'ecr') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
