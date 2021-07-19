pipeline {
  environment {
    registry = "novemberpain/spring-petclinic-hub"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.3.9'
    jdk 'jdk8'
  } 
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