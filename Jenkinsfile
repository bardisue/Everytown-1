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
        stage('Set File Permissions') {
            steps {
                sh 'chmod 644 default.conf'
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
