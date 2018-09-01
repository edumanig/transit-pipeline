pipeline {
  agent any
  stages {
    stage('Stage1') {
      parallel {
        stage('Build') {
          steps {
            addBadge(icon: 'Test Icon', text: 'Transit Switchover - Stage1')
          }
        }
        stage('fullcheck-before-upgrade') {
          steps {
            build(job: 'fullcheck-before-upgrade', propagate: true, wait: true, quietPeriod: 30)
          }
        }
        stage('notify slack') {
          steps {
            slackSend(baseUrl: 'https://aviatrix.slack.com/services/hooks/jenkins-ci/', message: 'Transit Pipeline - fullcheck before upgrade', channel: '#sitdown', failOnError: true, teamDomain: 'aviatrix', token: 'zjC6JXcuigU1Nq0j3AoLBdci')
          }
        }
      }
    }
    stage('Stage2') {
      parallel {
        stage('Upgrade') {
          steps {
            echo 'Stage Name - Upgrade'
            addInfoBadge 'Transit Switchover - Stage2 Upgrade'
          }
        }
        stage('transit-upgrade') {
          steps {
            build(job: 'transit-linux-ping', propagate: true, wait: true, quietPeriod: 10)
            build(job: 'transit-upgrade', propagate: true, quietPeriod: 30, wait: true)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline [Upgrade] --- Started', body: 'Upgrade, fullcheck-before-upgrade, transit-upgrade')
          }
        }
      }
    }
    stage('Stage3') {
      parallel {
        stage('Test1') {
          steps {
            addShortText 'Stage3 - Transit Switchover'
            addInfoBadge 'Transit Switchover - Stage3 '
          }
        }
        stage('transit-switchover') {
          steps {
            build(job: 'transit-switchover', propagate: true, wait: true, quietPeriod: 20)
            sleep 20
            build(job: 'ftp-test-only', propagate: true, wait: true, quietPeriod: 10)
            sleep 10
            build(job: 'transit-linux-ping', propagate: true, wait: true, quietPeriod: 10)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline Stage3', body: 'transit-switchover, ft-test-only', to: 'edsel@aviatrix.com')
            sleep 30
          }
        }
      }
    }
    stage('Stage4') {
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
            build(job: 'spoke-switchover', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('notify-me') {
          steps {
            mail(subject: 'transit-pipeline Stage4 [Test2]', body: 'force-peering-switchover, spoke-switchover', to: 'edsel@aviatrix.com')
            sleep 30
          }
        }
      }
    }
    stage('Stage5') {
      parallel {
        stage('Email') {
          steps {
            emailext(subject: 'Transit Pipeline  UserConnect-3.4.705- Passed 100%', body: 'Transit Switchover + Spoke Switchover', attachLog: true, to: 'dltest@aviatrix.com')
          }
        }
        stage('slack') {
          steps {
            slackSend(message: 'Transit Switchover -  UserConnect-3.4.703- Passed 100%', baseUrl: 'https://aviatrix.slack.com/services/hooks/jenkins-ci/', channel: '#sitdown', failOnError: true, teamDomain: 'aviatrix', token: 'zjC6JXcuigU1Nq0j3AoLBdci')
          }
        }
      }
    }
  }
}