pipeline {
  agent any
  stages {
    stage('Stage') {
      steps {
        tektonCreateRaw(inputType: 'FILE', input: '.tekton/pipeline-demo.yaml')
      }
    }
  }
}