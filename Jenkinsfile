pipeline {
  agent any
  stages {
    stage('Clonar repositorio') {
      steps {
        sh 'mkdir -p ~/.ssh'
        sh 'ssh-keyscan github.com >> ~/.ssh/known_hosts'
        sh 'git clone git@github.com:danielsanchez2024/product-api.git'
      }
    }

  }
}