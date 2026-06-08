pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-2"
        CLUSTER_NAME = "EKS-1"
        NAMESPACE = "webapps"
    }

    stages {

        stage('Configure kubeconfig') {
            steps {
                withCredentials([
                    string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')
                    ) {
                        sh """
                        aws eks update-kubeconfig \
                        --region $AWS_REGION \
                        --name $CLUSTER_NAME
                        """
                    }
                    }

        stage('Deploy To Kubernetes') {
            steps {
                sh "kubectl apply -f deployment-service.yml -n $NAMESPACE"
            }
        }

        stage('Verify Deployment') {
            steps {
                sh "kubectl get svc -n $NAMESPACE"
                sh "kubectl get pods -n $NAMESPACE"
            }
        }
    }
}
