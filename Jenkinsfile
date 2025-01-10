pipeline {
    agent any

    environment {
        // Usamos las credenciales de Jenkins para el usuario y la contraseña
        DOCKER_USER = credentials('docker-usuario')  // ID de la credencial de usuario en Jenkins
        DOCKER_PASSWORD = credentials('docker-password')  // ID de la credencial de contraseña en Jenkins
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                // Clona el repositorio desde Git
                sh 'git clone https://github.com/danielsanchez2024/product-api.git'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                // Construir la imagen Docker usando el Dockerfile
                sh 'docker build -t product-api:latest ./app'
            }
        }

        stage('Iniciar Sesión en Docker Registry') {
            steps {
                // Realizar login en Docker Registry usando las credenciales de Jenkins
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
                // Subir la imagen Docker construida al Docker Registry
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
