pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(credentialsId: 'ed', changelog: true, url: 'https://github.com/edumanig/cloudn.git', branch: 'master')
      }
    }
  }
}