pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://host.docker.internal:2375'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/bardisue/Everytown-1.git', branch: 'main'
            }
        }
        stage('Prepare Nginx Config') {
            steps {
                sh '''
                echo "server {
                    listen 80;
                    server_name localhost;
                    location / {
                        proxy_pass http://everytown:3000;
                    }
                }" > default.conf
                chmod 644 default.conf
                '''
            }
        }
        stage('Debug Info') {
            steps {
                sh '''
                pwd
                ls -la
                cat default.conf
                '''
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'docker-compose build'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'docker-compose down'
            }
        }
    }
}
