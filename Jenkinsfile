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
    }
    stages {
        stage("Run Image") {
            script {
                docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run("-p ${HOST_PORT}:${CONTAINER_PORT}")
            }
    }
}
