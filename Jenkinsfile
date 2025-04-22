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
               -Dsonar.java.binaries=. \
               -Dsonar.projectKey=mern_application'''
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
            slackSend channel: '#jenkins-slack',
            color: COLOR_MAP [currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URl}"     
        }
    }
}
