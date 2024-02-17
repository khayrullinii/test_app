pipeline {
  environment {
    imagename = "khayrullinii/app-nginx"
  }
  
  agent any
  parameters {
    choice choices: ['BRANCH_and_TAG'], description: '', name: 'TYPE'
    gitParameter (  branch: '', 
                branchFilter: '.*', 
                defaultValue: 'master', 
                description: '', 
                listSize: '10',
                name: 'BRANCH_and_TAG', 
                quickFilterEnabled: true, 
                selectedValue: 'NONE', 
                sortMode: 'NONE', 
                tagFilter: '*', 
                type: 'PT_BRANCH_TAG', 
                useRepository: 'https://github.com/khayrullinii/test_app/')
  }

  stages {
    stage('BRANCH_and_TAG') {
      when {
          expression { params.TYPE == 'BRANCH_and_TAG'}
      } 
      steps{
          checkout(
              [$class: 'GitSCM', 
              branches: [[name: "${params.BRANCH_and_TAG}"]], 
              doGenerateSubmoduleConfigurations: false, 
              extensions: [],
              submoduleCfg: [], userRemoteConfigs: 
              [[credentialsId: 'git',
              url: 'https://github.com/khayrullinii/test_app/']]]
              ) 
          }
    }
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
    stages {
    stage('Deploy kuber') {
    when {
          expression { params.BRANCH_and_TAG != "master" }
      } 
      steps{
        sh "echo 'test'"
      }
     }
    }
  }
}
