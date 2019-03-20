pipeline {
  agent any
  stages {
    stage('build') {
      parallel {
        stage('build') {
          steps {
            sleep 23
          }
        }
        stage('stuff2') {
          environment {
            dev = 'dev'
          }
          steps {
            input(message: 'do you want to move forward', submitter: 'dmin', submitterParameter: 'dmin', id: 'yes', ok: 'yes')
          }
        }
      }
    }
  }
}