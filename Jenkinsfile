pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/kalicharan-practice/aws-devops-flask.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f k8s/flask-deployment.yaml
                kubectl apply -f k8s/flask-service.yaml
                """
            }
        }

        stage('Restart Deployment (Rollout Update)') {
            steps {
                sh """
                kubectl rollout restart deployment flask-deployment
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                kubectl get pods
                kubectl get svc
                """
            }
        }

    }
}