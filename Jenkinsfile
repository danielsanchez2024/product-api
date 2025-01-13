pipeline {
    agent any

    environment {
        DOCKER_TOKEN = credentials('docker-token')
    }


    stages {

        stage('Limpiar Workspace') {
            steps {
                sh 'rm -rf product-api'
            }
        }

        stage('Clonar Repositorio') {
            steps {
                sh 'git clone -b main https://github.com/danielsanchez2024/product-api.git'
            }
        }

        stage('verificar version docker') {
            steps {
                sh 'docker --version'
            }
        }


        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t product-api:latest ./app'
            }
        }


        stage('Tag para Docker Registry') {
            steps {
                sh 'docker tag product-api:latest danielsanchez18/product-api:latest'
            }
        }


        stage('Iniciar Sesión en Docker Registry') {
            steps {
                 sh 'echo ${DOCKER_TOKEN} | docker login -u myusername --password-stdin'
            }
        }


        stage('Subir Imagen a Docker Registry') {
            steps {
                sh 'docker push danielsanchez18/product-api:latest'
            }
        }
    }

    post {
        success {
            echo 'La imagen fue construida y subida con éxito.'
        }
        failure {
            echo 'Hubo un error durante el proceso.'
        }
    }
}
