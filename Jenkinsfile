//Declarar o bloco pipeline
pipeline {
    //Declarar o agente que vai executar a pipeline. Como só tem uma máquina que está gerenciando (any)
    agent any

    //Estágios de execução da pipeline
    stages {

        stage ('Buil Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("cristianoalazaro/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}