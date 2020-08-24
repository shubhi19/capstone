pipeline {
  agent any
  stages {
    stage('Lint Dockerfile') {
      agent {
docker {
            image 'hadolint/hadolint:latest-debian'
        }
    }
      steps {
         sh 'hadolint Dockerfile'
      }
    }
    stage('Lint HTML'){
        steps{
            sh 'tidy -q -e templates/index.html'
        }
   }
    stage('Build Docker') {
      steps {
        sh 'make build'
      }
    }
    stage('Login to dockerhub') {
      steps {
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhubpwd')]) {
          sh 'docker login -u shubhi19 -p ${dockerhubpwd}'
        }
      }
    }
    stage('Upload Image') {
      steps {
        sh 'make upload'
      }
    }
    stage('Deploy Kubernetes') {
      steps {
        sh 'kubectl apply -f ./kubernetes'
      }
    }
  }
}
