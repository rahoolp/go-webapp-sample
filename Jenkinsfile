pipeline {
  agent any
  stages {
    stage('dev') {
      steps {
        sh 'go test ./...'
      }
    }

    stage('Build') {
      steps {
        script {
          app = docker.build("rahoolp/go-webapp-sample")
        }

      }
    }

  }
}