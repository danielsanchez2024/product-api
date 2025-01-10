pipeline {
    agent any

    environment {
        DOCKER_USER = credentials('docker-usuario')  // ID de la credencial de usuario en Jenkins
        DOCKER_PASSWORD = credentials('docker-password')  // ID de la credencial de contraseña en Jenkins
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                // Clona el repositorio desde la rama correcta
                sh 'git clone -b main https://github.com/danielsanchez2024/product-api.git'
            }
        }

        stage('verificar version docker') {
            steps {
                // Construir la imagen Docker
                sh 'docker --version'
            }
        }


        stage('Construir Imagen Docker') {
            steps {
                // Construir la imagen Docker
                sh 'docker build -t product-api:latest ./app'
            }
        }

        stage('Iniciar Sesión en Docker Registry') {
            steps {
                sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USER --password-stdin"
            }
        }

        stage('Tag para Docker Registry') {
            steps {
                sh 'docker tag product-api:latest danielsanchez18/product-api:latest'
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
