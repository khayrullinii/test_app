pipeline {
  environment {
    imagename = "khayrullinii/app-nginx"
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          sh 'docker build .'
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([string(credentialsId: 'dockerhub-pwd', passwordVariable: 'dockerhubpwd')]) {
                   sh 'docker login -u khayrullinii -p ${dockerhubpwd}'}
          sh 'docker push $imagename:latest"'
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
 
      }
    }
  }
}
