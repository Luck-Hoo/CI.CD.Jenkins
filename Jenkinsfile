pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nodejenkins"
        DOCKER_TAG = "latest"
        CONTAINER_PORT = "3000"
        HOST_PORT = "3000"
    }

    stages {
        stage("Build Image") {
            steps {
                script {
                    // Construa a imagem Docker
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f Dockerfile .")
                }
                echo "Inicializando o pipeline...."
            }
        }

        stage("Run Image") {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run("-p ${HOST_PORT}:${CONTAINER_PORT}")
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            script {
                // Comandos de limpeza do Docker
                try {
                    sh 'docker container prune -f'
                    sh 'docker image prune -f'
                } catch (Exception e) {
                    echo "Falha ao limpar: ${e}"
                }
            }
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
