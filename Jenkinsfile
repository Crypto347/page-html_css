pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'my-app'
        DOCKER_CONTAINER = 'my-app-container'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Crypto347/page-html_css.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh "docker run --rm ${DOCKER_IMAGE} ls /usr/share/nginx/html"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Remove container if it exists
                    sh "docker rm -f ${DOCKER_CONTAINER} || true"
                    // Run the new container
                    sh "docker run -d -p 8081:80 --name ${DOCKER_CONTAINER} ${DOCKER_IMAGE}"
                }
            }
        }
    }
}
