pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/zivhm/super-app.git'
            }
        }

        stage('Build and Install') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Test App') {
            steps {
                sh 'docker-compose up -d'
                sh 'sleep 30'
                sh 'curl http://localhost:3000/super-app'
                sh 'curl http://localhost:80'
            }
        }

        stage('Push') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('dockerhub-zivhm')
            }

            steps {
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
                sh 'docker-compose push'
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}
