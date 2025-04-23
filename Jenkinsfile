def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger'
    ]
pipeline {
   agent any
    environment {
     SCANNER_HOME = tool 'sonar-scanner'
    }
   stages {
      stage('git checkout') {
        steps {
            git 'https://github.com/Rohana-R/mern_application.git'  
        }
      }
      stage('code analysis') {
        steps {
            withSonarQubeEnv('sonar-server') {
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=mern_applicatio \
               -Dsonar.projectKey=mern_application'''
               }
        }
      }
      stage('docker push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                    sh 'docker tag pipelinemern-app-frontend rohana1234/pipelinemern-app-frontend'
                    sh 'docker tag pipelinemern-app-backend rohana1234/pipelinemern-app-backend'
                    sh 'docker push rohana1234/pipelinemern-app-frontend'
                    sh 'docker push rohana1234/pipelinemern-app-backend'    
                    }
                }
            }
        }
      stage('docker compose') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
   }
    post {
        always {
            echo 'slack Notification.'
            slackSend channel: '#sonarqube-mern',
            color: COLOR_MAP [currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URl}"     
        }
    }
}
