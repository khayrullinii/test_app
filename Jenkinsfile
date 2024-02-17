pipeline {
  environment {
    imagename = "khayrullinii/app-nginx"
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          sh 'docker build . -t $imagename:0.$BUILD_NUMBER'
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd',usernameVariable: 'dockerhubname', passwordVariable: 'dockerhubpwd')]) {
                   sh 'docker login -u ${dockerhubname} -p ${dockerhubpwd}'}
          sh 'docker push $imagename:0.$BUILD_NUMBER'
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:0.$BUILD_NUMBER"
 
      }
    }
    stage('Deploy to Kubernetes') {

            steps {
                script {
                    TAG_NAME = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()
                    sh('echo "Tag Name: ${TAG_NAME}"')                }
            }
        }
  }
}
