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

        //Outro estágio, fazendo o push da imagem para o registry do docker
        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        //Outro estágio, para o deploy
        stage ('Deploy Kubernetes') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                //credencial criada no Jenkins
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    //Executar comando no shell
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}