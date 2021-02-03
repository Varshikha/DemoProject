pipeline {
  environment {
    registry = "varshakashyap/apache_httpd"
    registryCredential = 'dockerhub'
  }
  agent {
        label 'master'
    }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Varshikha/DemoProject.git'
      }
    }
    stage('Building image') {
      steps{
        sh "docker build -t $registry:1.1 ."
        sh "docker run -d -p 8080:80 $registry:1.1"
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([usernamePassword( credentialsId: 'dockerhub', usernameVariable: 'USR', passwordVariable: 'PWD')]) {
          sh "docker login -u ${USR} -p ${PWD}"
          sh "docker push $registry:1.1"
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:1.1"
      }
    }
  }
}
