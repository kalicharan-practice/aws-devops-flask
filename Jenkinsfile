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

        stage('Verify Deployment') {
            steps {
                sh """
                echo "=== POD STATUS ==="
                kubectl get pods -o wide

                echo "=== SERVICE STATUS ==="
                kubectl get svc

                echo "=== DEPLOYMENT STATUS ==="
                kubectl get deployments

                echo "=== WAITING 30 SECONDS ==="
                sleep 30

                echo "=== FINAL POD STATUS ==="
                kubectl get pods

                echo "=== POD DESCRIBE ==="
                kubectl describe deployment flask-deployment
                """
            }
        }
    }
}