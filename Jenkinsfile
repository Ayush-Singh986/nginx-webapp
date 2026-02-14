pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ayush244/nginx-webapp"
        DOCKER_TAG = "latest"
        KUBECONFIG = "/home/jenkins/.kube/config"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ayush-Singh986/nginx-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:$DOCKER_TAG ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $DOCKER_IMAGE:$DOCKER_TAG"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f svc.yaml --validate=false
                kubectl apply -f hpa.yaml --validate=false
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh "kubectl get pods"
                sh "kubectl get svc"
                sh "kubectl get hpa"
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
