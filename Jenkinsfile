pipeline {
    agent any

    environment {
        IMAGE_NAME = "pokemon-api"
        CONTAINER_NAME = "pokemon-api-container"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Clonando el repositorio..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Construyendo la imagen Docker: ${IMAGE_NAME}"
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                echo "Desplegando la aplicación..."
                
                sh "docker stop ${CONTAINER_NAME} || true"
                
                sh "docker rm ${CONTAINER_NAME} || true"
                
                echo "Iniciando nuevo contenedor..."
                sh "docker run -d -p 8090:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                
                echo "Aplicación desplegada."
            }
        }
    }

    post {
        success {
            echo "Pipeline finalizado con éxito."
            echo "Accede a la aplicación en: http://<IP_DE_JENKINS>:8090/pokemons"
        }
        failure {
            echo "El pipeline ha fallado."
        }
    }
}