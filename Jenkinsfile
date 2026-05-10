pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/kalicharan-practice/aws-devops-flask.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop flask-container || true
                docker rm flask-container || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d --name flask-container -p 5000:5000 flask-app'
            }
        }

    }
}