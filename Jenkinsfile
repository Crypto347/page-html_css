pipeline {
    agent any
    stages {
        stage('Check Docker') {
            steps {
                script {
                    // Kiểm tra trạng thái của Docker
                    sh 'docker --version'
                    sh 'docker ps'
                }
            }
        }
    }
}
