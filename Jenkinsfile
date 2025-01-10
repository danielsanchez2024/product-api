pipeline {
    agent any

    environment {
        DOCKER_USER = credentials('docker-usuario')  // ID de la credencial de usuario en Jenkins
        DOCKER_PASSWORD = credentials('docker-password')  // ID de la credencial de contraseña en Jenkins
    }

    stages {
        stage('Limpiar Workspace') {
            steps {
                // Eliminar el directorio 'product-api' si existe
                sh 'rm -rf product-api'
            }
        }
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                // Clona el repositorio desde Git
                echo 'Clonando el repositorio...'
                sh 'git clone -b main https://github.com/danielsanchez2024/product-api.git'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                // Verificar si la carpeta ./app existe
                echo 'Construyendo la imagen Docker...'
                sh 'ls -al ./app'  // Mostrar contenido de la carpeta ./app para verificar que existe
                sh 'docker build -t product-api:latest ./app'
            }
        }

        stage('Iniciar Sesión en Docker Registry') {
            steps {
                echo 'Iniciando sesión en Docker Registry...'
                // Realizar login en Docker Registry usando las credenciales de Jenkins
                sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USER --password-stdin"
            }
        }

        stage('Tag para Docker Registry') {
            steps {
                echo 'Aplicando tag a la imagen Docker...'
                sh 'docker tag product-api:latest danielsanchez18/product-api:latest'
            }
        }

        stage('Subir Imagen a Docker Registry') {
            steps {
                echo 'Subiendo la imagen Docker al Registry...'
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
