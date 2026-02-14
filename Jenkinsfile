pipeline {
    agent any

    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f svc.yaml
                kubectl apply -f hpa.yaml
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
                sh 'kubectl get hpa'
            }
        }
    }
}
