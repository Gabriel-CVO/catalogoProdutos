pipeline {
    agent any
    stages {
        stage('Verificar Repositório') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], useRemoteConfigs:[[url: 'https://github.com/Gabriel-CVO/catalogoProdutos.git']]])
            }
        }

        stage('Construir Imagem Docker') {
            steps {
                script {
                    def appName = 'av1'
                    def imageTag = "${appName}:${env.BUILD_ID}"

                    // Construir a imagem Docker
                    bat "docker build -t ${imageTag} ."
                }
            }
        }

        stage('Fazer Deploy') {
            steps {
                script {
                    def appName = 'av1'
                    def imageTag = "${appName}:${env.BUILD_ID}"
                    // Parar e remover o container existente, se houver
                    bat "docker stop ${appName} || exit 0"
                    bat "docker rm ${appName} || exit 0"
                    // Executar o novo container
		    bat "docker-compose up -d --build"
                }
            }
        }
    }
    post {
        success {
            echo 'Deploy realizado com sucesso!'
        }
        failure {
            echo 'erro durante o deploy.'
        }
}
