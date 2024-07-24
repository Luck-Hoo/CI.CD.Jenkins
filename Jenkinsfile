pipeline {
    agent any

    stages {
        stage("Build Image") {
            steps {
                script {
                    // Construa a imagem Docker
                    docker.build("nodejenkins", "-f Dockerfile .")
                }
                echo "Inicializando o pipeline...."
            }
        }
    }
}
