pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'kube8', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://BEF0551737BF4D3CACF637B6BB7CFE68.gr7.ap-south-2.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yaml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'kube8', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://BEF0551737BF4D3CACF637B6BB7CFE68.gr7.ap-south-2.eks.amazonaws.com']]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}
