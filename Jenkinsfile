pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        bat 'build/libs/**/*.jar'
        bat 'build/docs/**'
        bat 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'lolol', to: 'gi_berkane@esi.dz', from: 'gi_berkane@esi.dz', body: 'ss', replyTo: 'gi_berkane@esi.dz')
      }
    }

    stage('Code Analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          bat 'gradle sonarqube'
        }

        waitForQualityGate true
      }
    }

  }
}