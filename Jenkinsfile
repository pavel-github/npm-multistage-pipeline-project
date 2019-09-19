#!/usr/bin/env groovy

pipeline {
  agent {
    label 'nodejs'
  }

  stages {

    stage('Prepare') {
      steps {
        script {
          sh 'echo "INITIALIZE PIPELINE"'
        }

        dir('comet') {
          sh 'echo "Comet Dependencies"'
          sh 'npm install'
        }

        dir('lambda') {
          sh 'echo "Lambda Dependencies"'
          sh 'npm install'
        }
      }
    }

    stage('Check Comet') {
      steps {
        dir('comet') {
          sh 'echo "==> Snyk Comet"'
          snykSecurity projectName: 'comet',
            snykInstallation: 'snyk@latest',
            snykTokenId: 'snyk-mxu-token',
            targetFile: 'package.json'

          sh 'echo "==> SonarQube Tests"'
        }
      }
    }

    stage('Check Lambda') {
      steps {
        dir('lambda') {
          sh 'echo "==> Snyk Lambda"'
          snykSecurity projectName: 'lambda',
            snykInstallation: 'snyk@latest',
            snykTokenId: 'snyk-mxu-token',
            targetFile: 'package.json'

          sh 'echo "==> SonarQube Tests"'
        }
      }
    }

    stage('Validate') {
      steps {
        sh 'echo "==> Validate AWS templates"'
      }
    }
  }
}
