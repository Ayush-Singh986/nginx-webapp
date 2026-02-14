pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Ayush-Singh986/nginx-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ayushsinghdev/nginx-webapp:v1 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker login -u ayushsinghdev -p YOUR_DOCKER_PASSWORD'
                sh 'docker push ayushsinghdev/nginx-webapp:v1'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl apply -f k8s/hpa.yaml'
            }
        }
    }
}
