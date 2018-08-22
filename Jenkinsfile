pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            addBadge(icon: 'Test Icon', text: 'hello world')
          }
        }
        stage('fullcheck-before-upgrade') {
          steps {
            build(job: 'fullcheck-before-upgrade', propagate: true, wait: true)
          }
        }
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
            build(job: 'transit-upgrade', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline [Upgrade] --- Passed 100%', body: 'Upgrade, fullcheck-before-upgrade, transit-upgrade', to: 'edsel@aviatrix.com', from: 'noreply@aviatrix.com')
            sleep 30
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
            build(job: 'transit-switchover', propagate: true, wait: true, quietPeriod: 20)
            sleep 20
            build(job: 'ftp-test-only', propagate: true, wait: true)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline [Test1]', body: 'transit-switchover, ft-test-only', to: 'edsel@aviatrix.com')
            sleep 30
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
            build(job: 'force-peering-switchover', propagate: true, wait: true, quietPeriod: 20)
            sleep 20
            build(job: 'spoke-switchover', propagate: true, wait: true)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline [Test2]', body: 'force-peering-switchover, spoke-switchover', to: 'edsel@aviatrix.com')
            sleep 30
          }
        }
      }
    }
    stage('Email') {
      steps {
        emailext(subject: 'Transit Pipeline  - Passed 100%', body: 'Transit Switchover + Spoke Switchover', attachLog: true, to: 'edsel@aviatrix.com, ')
      }
    }
  }
}