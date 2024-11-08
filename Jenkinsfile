pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'myapp'
        CONTAINER_NAME = 'myapp-container'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Crypto347/page-html_css.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE).push("latest")
                }
            }
        }
        stage('Deploy to Docker') {
            steps {
                sh "docker run -d --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:latest"
            }
        }
    }
}
