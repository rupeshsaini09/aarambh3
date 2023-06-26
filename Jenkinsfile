pipeline {
  environment {
    dockerimagename = "rupeshsaini09/aarambh3"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Pulling Latest Code From GitHub') {
      steps {
        git 'https://github.com/rupeshsaini09/aarambh3.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
               registryCredential = 'docker-hub-creds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying webapp into container') {
      steps {
        script {
          def output = sh(script: "$(\docker container ls -q)", returnStdout: true)
          docker container rm --force "${output}"
          sh "docker container run -d -p 80:80 rupeshsaini09/aarambh3"
        }
      }
    }
  }
}
