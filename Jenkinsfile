pipeline {
    agent any

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t ayush244/nginx-webapp:v1 .'
            }
        }

        stage('Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    docker push ayush244/nginx-webapp:v1
                    '''
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f svc.yaml
                kubectl apply -f hpa.yaml
                '''
            }
        }
    }
}
