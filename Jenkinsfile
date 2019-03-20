pipeline {
  agent none
  stages {
    stage('build') {
      agent any
      steps {
        sleep 2
      }
    }
    stage('approval') {
      steps {
        input(message: 'approved', id: 'approve', ok: 'YES', submitter: 'dmin', submitterParameter: 'YES')
        catchError() {
          input(message: 'new', id: 'hh', ok: 'jj', submitter: 'dmin', submitterParameter: 'nj')
        }

      }
    }
  }
}