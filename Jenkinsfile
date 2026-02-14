pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ayushsinghdev/nginx-webapp:v1 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push ayushsinghdev/nginx-webapp:v1'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
