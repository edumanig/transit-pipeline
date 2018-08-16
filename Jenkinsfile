pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        addBadge(icon: 'Test Icon', text: 'hello world')
      }
    }
    stage('Upgrade') {
      parallel {
        stage('Upgrade') {
          steps {
            echo 'Stage Name - Upgrade'
          }
        }
        stage('transit-upgrade') {
          steps {
            build(job: 'transit-upgrade', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Report') {
      parallel {
        stage('Report') {
          steps {
            echo 'Archive Results'
          }
        }
        stage('send email') {
          steps {
            emailext(attachLog: true, subject: 'transit-pipeline results', body: 'upgrade', from: 'noreply@aviatrix.com', to: 'edsel@aviatrix.com')
          }
        }
      }
    }
  }
}