#!/usr/bin/env groovy

pipeline {
  agent {
    label 'nodejs'
  }

  stages {
    stage('Checkout') {
      steps {
        deleteDir()
        git 'https://github.com/pavel-github/npm-multistage-pipeline-project'
      }
    }

    stage('Prepare') {
      steps {
        script {
          sh 'echo "INITIALIZE PIPELINE"'
        }

        dir('comet') {
          sh 'echo "Comet Dependencies"'
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

    stage('Validate') {
      steps {
        sh 'echo "==> Validate AWS templates"'
      }
    }
  }
}
