@Library('jenkins-helpers@master')_

pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        buildStep()
      }
    }
    stage('Deploy') {
      // run this stage only when on master in the original repository and build is successful
      when {
        environment name: 'CHANGE_FORK', value: ''
        expression { GIT_URL ==~ 'https://github.com/sw4j-org/.*' }
        expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
      }
      steps {
        doDeploy()
        publishSite()
      }
    }
  }
  post {
    always {
      resultMailer()
      cleanWs()
    }
  }
}
