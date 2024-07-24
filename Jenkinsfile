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
                    // Verifica se a porta está livre
                    def portInUse = bat(script: "netstat -an | findstr :${HOST_PORT}", returnStatus: true) == 0
                    if (portInUse) {
                        echo "A porta ${HOST_PORT} já está em uso. Tentando parar o processo na porta..."
                        // Encontre o PID do processo usando a porta e pare-o
                        def pid = bat(script: "netstat -aon | findstr :${HOST_PORT} | findstr LISTENING", returnStdout: true).trim().split()[4]
                        bat "taskkill /PID ${pid} /F"
                    }

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
                        bat "docker stop ${env.CONTAINER_ID}"
                        bat "docker rm ${env.CONTAINER_ID}"
                    } catch (Exception e) {
                        echo "Falha ao parar o container: ${e}"
                    }
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
