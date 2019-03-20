pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sleep 2
      }
    }
    stage('approval') {
      steps {
        input(message: 'approved', id: 'approve', ok: 'YES', submitter: 'dmin', submitterParameter: 'YES')
      }
    }
  }
}