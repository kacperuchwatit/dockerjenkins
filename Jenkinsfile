pipeline {
  agent {
    docker {
      image 'maven:3.8.1-openjdk-11'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          docker.withRegistry('https://943696080604.dkr.ecr.us-west-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
            def customImage = docker.build("943696080604.dkr.ecr.us-west-1.amazonaws.com/dockerjenkins:${env.BUILD_ID}")
            customImage.push()
          }
        }
      }
    }
    stage('Deploy') {
      steps {
      }
    }
  }
}
