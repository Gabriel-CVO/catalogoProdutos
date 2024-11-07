pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3' // Certifique-se de que esta versão do Maven esteja instalada no Jenkins
        jdk 'JDK 17'
    }

    environment {
        GIT_REPOSITORY = 'https://github.com/Gabriel-CVO/catalogoProdutos.git'
        DOCKER_IMAGE = "catalogo-produtos:${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${env.GIT_REPOSITORY}"
            }
        }

        stage('Build') {
            steps {
                bat './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                bat './mvnw test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Adicione os comandos específicos de deploy para o ambiente Windows
            }
        }
    }

    post {
        success {
            echo 'Build and deployment completed successfully!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}