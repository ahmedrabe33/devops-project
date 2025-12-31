pipeline{
    agent any
    enviroment {
        DOCKER_IMAGE = "my-web-page:latest"
    }
    stages {
        stage('checkout') {
            steps {
                git branch : 'main' , url: https://github.com/ahmedrabe33/devops-project.git
            }
        }
        stage('build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        stege(DEPLOY) {
            steps {
                sh 'docker compose down || true '
                sh 'docker compose up -d' 
            }
        }
    }
}