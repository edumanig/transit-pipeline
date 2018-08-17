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
        stage('fullcheck-before-upgrade') {
          steps {
            build(job: 'fullcheck-before-upgrade', propagate: true, wait: true)
          }
        }
        stage('transit-upgrade') {
          steps {
            build(job: 'transit-upgrade', propagate: true)
          }
        }
      }
    }
    stage('Test1') {
      parallel {
        stage('Test1') {
          steps {
            addShortText 'Stage1 - Transit Switchover'
          }
        }
        stage('transit-switchover') {
          steps {
            build(job: 'transit-switchover', propagate: true, wait: true)
          }
        }
        stage('ftp-test-only') {
          steps {
            build(job: 'ftp-test-only', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Test2') {
      parallel {
        stage('Test2') {
          steps {
            echo 'Stage2'
          }
        }
        stage('force-peering-switchover') {
          steps {
            build(job: 'force-peering-switchover', propagate: true, wait: true)
          }
        }
        stage('spoke-switchover') {
          steps {
            build(job: 'spoke-switchover', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Email') {
      steps {
        emailext(subject: 'Transit Pipeline  - Passed 100%', body: 'Transit Switchover + Spoke Switchover', attachLog: true, to: 'edsel@aviatrix.com')
      }
    }
  }
}