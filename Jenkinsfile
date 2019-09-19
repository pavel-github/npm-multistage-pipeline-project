#!/usr/bin/env groovy

pipeline {
  agent {
    label 'nodejs'
  }
​
  options {
    timestamps()
    timeout(time: 2, unit: 'HOURS')
  }
​
  stages {
    stage('Prepare') {
      steps {
        script {
          sh "echo 'INITIALIZE PIPELINE'"
        }
​
        dir('comet') {
          sh '''
              echo "Comet Dependencies"
              npm install
          '''
        }
      }
    }
​
    stage('Check Comet') {
      steps {
        dir('comet') {
          sh 'echo "==> Snyk Comet"'
          snykSecurity projectName: 'comet',
            snykInstallation: 'snyk@latest',
            snykTokenId: 'snyk-mxu-token',
            targetFile: 'package.json'
​
          sh 'echo "==> SonarQube Tests"'
        }
      }
    }
​​
    stage('Validate') {
      steps {
        sh """
          echo '==> Validate AWS templates'
        """
      }
    }
  }
}
