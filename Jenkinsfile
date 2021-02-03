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
        sh "docker build -t $registry:$BUILD_NUMBER ."
      }
    }
    stage('Pushing image to dockerhub registry') {
      steps{
        script {
          withCredentials([usernamePassword( credentialsId: 'dockerhub', usernameVariable: 'USR', passwordVariable: 'PWD')]) {
          sh "docker login -u ${USR} -p ${PWD}"
          sh "docker push $registry:$BUILD_NUMBER"
          }
        }
      }
    }
    stage('Deploying image') {
      steps{
        sh "docker run -d -p 8080:80 $registry:$BUILD_NUMBER"
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
