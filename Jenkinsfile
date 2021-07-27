pipeline {
  environment {
    registry = "novemberpain/spring-petclinic-hub"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent { label: 'worker' }  
  stages {
     stage('Clone Git') {
       steps {
         git 'https://github.com/november-pain/spring-petclinic.git'
       }
     }
    stage('Compile') {
       steps {
         sh './mvnw package' 
       }
    }
    stage('Test') {
      steps {
        sh 'echo test'
      }
    }
    stage('Build Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:latest"
      }
    }
  }
}
