pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'my-app'
        DOCKER_CONTAINER = 'my-app-container'
        SONAR_TOKEN = credentials('micro-sonar-token') 
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
          stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'  // Kiểm tra tên công cụ đã được cấu hình chính xác trong Jenkins
                    withSonarQubeEnv('sonarqube') {  // Đảm bảo tên cấu hình SonarQube là 'sonarqube'
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=micro \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://192.168.1.40:9000 \
                        -Dsonar.login=<micro-sonar-token>  // Thay <micro-sonar-token> bằng token thực tế
                        """
                    }
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
