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
            jacoco()
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'gradle uploadArchives'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(sendAsText: true, baseUrl: 'https://hooks.slack.com/services/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', channel: '#ss', color: '#ff0000', failOnError: true, message: 'build sucessfull', token: 'TSRL3TFPC/BTE4Z9CAJ/fZFbSAvP17sy4tCxRnTjNnro', teamDomain: 'outilshq', username: 'gi_berkane')
      }
    }

  }
}