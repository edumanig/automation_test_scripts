pipeline {
  agent any
  stages {
    stage('Upgrade Controller') {
      parallel {
        stage('Prepare Controller') {
          steps {
            addHtmlBadge 'Upgrade controller to latest 4.0'
          }
        }
        stage('Upgrade') {
          steps {
            build(job: 'upgrade_only', propagate: true, quietPeriod: 10, wait: true)
          }
        }
      }
    }
    stage('Acceptance Test') {
      parallel {
        stage('Acceptance Test') {
          steps {
            addHtmlBadge 'Run terraform acceptance test'
          }
        }
        stage('error') {
          steps {
            build(job: 'tr-acceptance', propagate: true, quietPeriod: 10, wait: true)
          }
        }
      }
    }
    stage('Report') {
      parallel {
        stage('Report') {
          steps {
            addHtmlBadge 'Send reports'
          }
        }
        stage('error') {
          steps {
            timestamps()
            slackSend(baseUrl: 'https://aviatrix.slack.com/services/hooks/jenkins-ci/', channel: '#terraform-acceptance', failOnError: true, teamDomain: 'aviatrix', token: 'zjC6JXcuigU1Nq0j3AoLBdci', message: 'Terraform Acceptance 4.0 - Passed')
          }
        }
      }
    }
  }
}