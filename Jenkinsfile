pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/Rohana-R/mern_application.git'
            }
        }
        stage('docker compose') {
            steps {
                script{
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
