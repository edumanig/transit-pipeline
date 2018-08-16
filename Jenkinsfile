pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/edumanig/cloudn.git', poll: true, changelog: true, credentialsId: 'edumanig', branch: 'master')
      }
    }
  }
}