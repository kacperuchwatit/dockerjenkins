pipeline {
    agent any
    environment {
        registry = "943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/jenkins-ecr']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/kacperuchwatit/dockerjenkins']]])     
            }
        }
  
    
    stage('Building image') {
      steps{
        script {
            sh 'ssh -i /var/jenkins_home/workspace/Docker\ AWS/Jenkins-KeyPair.pem ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "dockerImage = docker.build registry"' 
        }
      }
    }
   
    
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'https://943696080604.dkr.ecr.us-west-1.amazonaws.com', 'ecr:us-west-1:aws-credentials'
                sh 'ssh -i /var/jenkins_home/workspace/Docker\ AWS/Jenkins-KeyPair.pem ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "docker push 943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:latest"'
         }
        }
      }
   
         
     stage('stop previous containers') {
         steps {
            sh 'ssh -i /var/jenkins_home/workspace/Docker\ AWS/Jenkins-KeyPair.pem ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop"'
            sh 'ssh -i /var/jenkins_home/workspace/Docker\ AWS/Jenkins-KeyPair.pem ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm"'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'ssh -i /var/jenkins_home/workspace/Docker\ AWS/Jenkins-KeyPair.pem ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "docker run -d -p 8096:5000 --rm --name mypythonContainer 943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:latest"'
            }
      }
    }
    }
}
