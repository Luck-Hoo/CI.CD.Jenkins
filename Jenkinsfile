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
                    // Execute a imagem Docker e armazene o ID do container
                    def containerId = docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run("-d -p ${HOST_PORT}:${CONTAINER_PORT}").id
                    env.CONTAINER_ID = containerId
                    echo "Container iniciado com ID: ${env.CONTAINER_ID}"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            script {
                // Pare o container
                if (env.CONTAINER_ID) {
                    try {
                        sh "docker stop ${env.CONTAINER_ID}"
                        sh "docker rm ${env.CONTAINER_ID}"
                    } catch (Exception e) {
                        echo "Falha ao parar o container: ${e}"
                    }
                }

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
