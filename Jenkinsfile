pipeline {
    agent any

    stages {
        stage("Build Image") {
            steps {
                script {
                    // Construa a imagem Docker
                    docker.build("NodeComJenkins", "-f ./src/Dockerfile ./src")
                }
                echo "Inicializando o pipeline...."
            }
        }
    }
}
