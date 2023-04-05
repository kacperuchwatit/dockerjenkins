pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                    sh 'scp -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem Dockerfile ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com:/home/ec2-user'
                 sh 'ssh -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem -T ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "sudo docker build -t dockerjenkins ."'
                }
            }
        }
        stage('Tag') { 
            steps { 
                script{
                 sh 'ssh -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem -T ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "sudo docker tag dockerjenkins:latest 943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:("${env.BUILD_NUMBER}")"'
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                script{
                     sh 'ssh -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem -T ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "aws ecr get-login-password --region us-west-1 | sudo docker login --username AWS --password-stdin 943696080604.dkr.ecr.us-west-1.amazonaws.com"'
                     sh 'ssh -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem -T ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "sudo docker push 943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:latest"'
                    }
                }
            }
        }
    }
