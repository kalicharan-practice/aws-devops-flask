pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kalicharandas001/aws-devops-flask-app"
        TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kalicharan-practice/aws-devops-flask.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE:$TAG .
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS')]) {

                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    """
                }
            }
        }

        stage('Push Image') {
            steps {
                sh """
                docker push $DOCKER_IMAGE:$TAG
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f k8s/
                kubectl rollout restart deployment flask-deployment
                """
            }
        }
    }
}