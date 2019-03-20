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
    stage('build') milestone() {
      agent any
      steps {
        sleep 2
      }
 lock(resource: 'myResource', inversePrecedence: true)
    }
    stage('approval') milestone() {
      steps {
        catchError() {
          input(message: 'approve', id: 'approve', ok: 'YES', submitter: 'dmin', submitterParameter: 'YES')
        }

      }
    }
  }
}
