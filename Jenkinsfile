  [
    $class: 'ThrottleJobProperty',
    categories: ['my_category'],
    limitOneJobWithMatchingParams: false,
    maxConcurrentPerNode: 2,
    maxConcurrentTotal: 2,
    paramsToUseForLimit: '',
    throttleEnabled: true,
    throttleOption: 'category'
  ]
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
        catchError() {
          input(message: 'approve', id: 'approve', ok: 'YES', submitter: 'dmin', submitterParameter: 'YES')
        }

      }
    }
  }
}
