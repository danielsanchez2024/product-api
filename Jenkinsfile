pipeline {
  agent any
  stages {
    stage('Clonar repositorio') {
      steps {
        sh 'mkdir -p ~/.ssh'
        sh 'ssh-keyscan github.com >> ~/.ssh/known_hosts'
      }
    }

  }
}