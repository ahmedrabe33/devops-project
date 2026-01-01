pipeline {
    agent { label 'aws-agent'}

    environment {
        DOCKER_IMAGE = "my-web-page:latest"
    }

    stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedrabe33/devops-project.git'
            }
        }

        stage('build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('DEPLOY') {
            steps {
                script {
                    sh 'docker compose down || true'
                    sh 'docker compose up -d'
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sh 'docker ps --filter name=my-web-container'
                    sh 'curl -I http://localhost:8081'
                }
            }
        }
    }

    // إضافة قسم التنبيهات لربط النتائج بـ Slack
    post {
        success {
            slackSend(
                channel: '#jenkins-ci', 
                color: 'good', 
                message: "✅ *Deployment Successful!* \n*Project:* ${env.JOB_NAME} \n*Build Number:* ${env.BUILD_NUMBER} \n*App URL:* http://localhost:8081"
            )
        }
        failure {
            slackSend(
                channel: '#jenkins-ci', 
                color: 'danger', 
                message: "❌ *Deployment Failed!* \n*Project:* ${env.JOB_NAME} \n*Build Number:* ${env.BUILD_NUMBER} \n*Check Jenkins Console:* ${env.BUILD_URL}console"
            )
        }
    }
}