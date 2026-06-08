pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-2"
        CLUSTER_NAME = "EKS-1"
        NAMESPACE = "webapps"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Madhu-gt-hb/SampleDemo.git'
            }
        }

        stage('Configure kubeconfig') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws'
                ]]) {
                    sh '''
                        aws sts get-caller-identity

                        aws eks update-kubeconfig \
                            --region $AWS_REGION \
                            --name $CLUSTER_NAME
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "kubectl apply -f deployment-service.yml -n $NAMESPACE"
            }
        }

        stage('Verify') {
            steps {
                sh "kubectl get pods -n $NAMESPACE"
                sh "kubectl get svc -n $NAMESPACE"
            }
        }
    }
}
