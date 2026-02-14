pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ayush244/nginx-webapp"
        DOCKER_TAG = "latest"
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
                sh '''
                echo "YOUR_DOCKER_PASSWORD" | docker login -u ayush244 --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $DOCKER_IMAGE:$DOCKER_TAG"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f svc.yaml --validate=false
                kubectl apply -f hpa.yaml --validate=false
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                kubectl get pods
                kubectl get svc
                kubectl get hpa
                '''
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
