#!/usr/bin/env groovy
pipeline {
  agent {
    docker {
      image 'browsers'
    }
  }

  environment {
    GIT_SSL_NO_VERIFY = true
  }

  stages {


    stage('Build project') {

      steps {
        echo "Building ${env.BUILD_ID}"
        sh 'mkdir -p reports'
        sh 'npm install'
        sh 'npm run e2edocker'
        publishHTML target: [allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'reports/combined', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '']


      }
    }
    stage('Run  e2e Tests') {
      steps {

        steps {
          echo "Running e2e Tests with TestCafe and CucumberJS"
          sh 'npm run e2edocker'

        }
      }
    }
  }
  post {
    always {
      echo 'Publishing HTML reports'


      // Close open browsers
      sh 'pkill -f chrome || true'
      sh 'pkill -f firefox || true'

    }
    failure {
      echo 'Build failed!'

    }
  }
}
