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
                 sh 'ssh -i /var/jenkins_home/workspace/trigger/Jenkins-KeyPair.pem -T ec2-user@ec2-13-57-229-166.us-west-1.compute.amazonaws.com "sudo docker run -d hello-world"'
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
                        docker.withRegistry('https://943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins', 'ecr:us-west-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
