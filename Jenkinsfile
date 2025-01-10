pipeline {
    agent any

    environment {
        // Variables de entorno
        IMAGE_NAME = 'mi-imagen'
        DOCKER_REGISTRY = 'mi-registro.com'
        DOCKER_USER = credentials('docker-usuario') // Usa las credenciales guardadas en Jenkins
        DOCKER_PASSWORD = credentials('docker-password')
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                // Clona el repositorio desde Git
                sh 'git clone git@github.com:danielsanchez2024/product-api.git'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                // Construir la imagen Docker usando el Dockerfile
                sh 'docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest .'
            }
        }

        stage('Iniciar Sesión en Docker Registry') {
            steps {
                // Realizar login en el Docker Registry (usando las credenciales guardadas)
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin $DOCKER_REGISTRY'
            }
        }

        stage('Subir Imagen a Docker Registry') {
            steps {
                // Subir la imagen Docker construida al Docker Registry
                sh 'docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest'
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
