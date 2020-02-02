pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/**'
        junit 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'lolol', to: 'gi_berkane@esi.dz', from: 'gi_berkane@esi.dz', body: 'ss', replyTo: 'gi_berkane@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonarqube'
            }

            waitForQualityGate true
          }
        }

        stage('Test reporting') {
          steps {
            jacoco(execPattern: 'build/jacoco/*.exec', exclusionPattern: '**/test/*.class')
          }
        }

      }
    }

    stage('Deployment') {
      when {
        branch 'master'
      }
      steps {
        bat 'gradle uploadArchives'
      }
    }

    stage('Slack Notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(sendAsText: true, baseUrl: 'https://hooks.slack.com/services', channel: '#ss', color: '#ff0000', failOnError: true, message: 'build sucessfull', token: 'TSRL3TFPC/BTE4Z9CAJ/BjaB3VmwTFCZ3vcBYDyPltzd', teamDomain: 'outilshq', username: 'gi_berkane')
      }
    }

  }
}