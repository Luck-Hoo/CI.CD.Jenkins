pipeline {
    agent any

    environment {
        // Variáveis de ambiente, se necessário
        DOCKER_IMAGE = 'nodejenkins'
        DOCKER_TAG = 'latest'
        CONTAINER_PORT = '3000'
        HOST_PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout do código-fonte
                git 'https://github.com/Luck-Hoo/CI.CD.Jenkins.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Construa a imagem Docker
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f Dockerfile .")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Execute o contêiner Docker
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run("-p ${HOST_PORT}:${CONTAINER_PORT}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Aqui você pode adicionar etapas de teste, se necessário
                    echo 'Run application tests...'
                }
            }
        }
    }

    post {
        always {
            // Limpeza, como remover contêineres antigos
            echo 'Cleaning up...'
            sh 'docker container prune -f'
            sh 'docker image prune -f'
        }

        success {
            // Notificações de sucesso
            echo 'Pipeline succeeded!'
        }

        failure {
            // Notificações de falha
            echo 'Pipeline failed!'
        }
    }
}
