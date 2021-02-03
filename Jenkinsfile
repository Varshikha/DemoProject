pipeline {
  environment {
    registry = "varshakashyap/apache_httpd"
    registryCredential = 'dockerhub'
  }
  agent {
        label 'developers-lab'
    }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Varshikha/DemoProject.git'
      }
    }
    stage('Building image') {
      steps {
        sh "docker build -t $registry:$BUILD_NUMBER ."
      }
    }
    stage('Pushing image to dockerhub registry') {
      steps {
        script {
          withCredentials([usernamePassword( credentialsId: 'dockerhub', usernameVariable: 'USR', passwordVariable: 'PWD')]) {
          sh "docker login -u ${USR} -p ${PWD}"
          sh "docker push $registry:$BUILD_NUMBER"
          }
        }
      }
    }
    stage('Deploying image') {
      steps {
        script {
          withKubeConfig([credentialsId: 'APILoginVUser', serverUrl: 'https://lb.coe.com/k8s/clusters/c-thsfs']) {
          sh 'kubectl version --short'
          sh "BUILD_NUMBER=${BUILD_NUMBER} && envsubst < demoproject-deploy.yaml > apachehttp.yaml"
          sh 'kubectl apply -f apachehttp.yaml'
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
