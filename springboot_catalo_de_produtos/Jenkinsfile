pipeline {
    agent any

    stages {
        stage('Verificar Repositório') {
            steps {
                bat 'git clone https://github.com/Gabriel-CVO/springboot_catalo_de_produtos.git'
            }
        }

        stage('Instalar Dependências') {
            steps {
                script {
                    // Navegar até o diretório do projeto
                    dir('springboot_catalo_de_produtos') {
                        // Instalar dependências usando Maven
                        bat 'mvn clean install'
                    }
                }
            }
        }

        stage('Construir Imagem Docker') {
            steps {
                script {
                    def appName = 'springboot_catalogo_de_produtos'
                    def imageTag = "${appName}:${env.BUILD_ID}"

                    // Construir a imagem Docker (em um ambiente Windows)
                    bat "docker build -t ${imageTag} ."
                }
            }
        }

        stage('Fazer Deploy') {
            steps {
                script {
                    def appName = 'springboot_catalogo_de_produtos'
                    def imageTag = "${appName}:${env.BUILD_ID}"

                    // Parar e remover o container existente, se houver
                    bat "docker stop ${appName} || exit 0"
                    bat "docker rm ${appName} || exit 0"

                    // Executar o novo container
                    bat "docker run -d --name ${appName} -p 8090:8090 ${imageTag}"
                }
            }
        }
    }

    post {
        success {
            echo 'Deploy realizado com sucesso!'
        }
        failure {
            echo 'Houve um erro durante o deploy.'
        }
    }
}