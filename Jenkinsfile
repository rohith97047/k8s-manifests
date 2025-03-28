pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        EKS_CLUSTER = 'my-eks-cluster'
        KUBECONFIG_PATH = '~/.kube/config'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo/your-k8s-app.git'
            }
        }

        stage('Configure AWS Credentials') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    sh 'aws eks update-kubeconfig --region ${AWS_REGION} --name ${EKS_CLUSTER}'
                }
            }
        }

        stage('Deploy to AWS EKS Fargate') {
            steps {
                sh '''
                kubectl apply -f k8s-manifests/namespace.yaml
                kubectl apply -f k8s-manifests/deployment.yaml
                kubectl apply -f k8s-manifests/service.yaml
                '''
            }
        }
    }
}
