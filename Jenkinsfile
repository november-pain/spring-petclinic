pipeline {
  environment {
    registry = "novemberpain/spring-petclinic-hub"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
     stage('Cloning Git') {
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
        sh '''
        mvnw clean install
        ls
        pwd
        ''' 
      }
    }
    stage('Building Image') {
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
