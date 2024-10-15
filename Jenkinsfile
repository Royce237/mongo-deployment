pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/Royce237/mongo-deployment.git'
        NAMESPACE_STAGE = 'stage'
        NAMESPACE_DEV = 'dev'
        NAMESPACE_PROD = 'prod'
        K8S_CLUSTER = 'default'
        K8S_TOKEN = credentials('k8s-service-account-token') // Use Jenkins credential ID
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Deploy to Stage') {
            steps {
                script {
                    // Helm install/upgrade in stage namespace
                    sh """
                        export KUBECONFIG=/path/to/your/kubeconfig
                        helm upgrade --install backend backend --namespace ${NAMESPACE_STAGE} --create-namespace --kube-token ${K8S_TOKEN}
                        helm upgrade --install frontend frontend --namespace ${NAMESPACE_STAGE} --create-namespace --kube-token ${K8S_TOKEN}
                    """
                }
            }
        }

        // Similar for other stages...
    }

    post {
        always {
            echo "Deployment pipeline completed."
        }
    }
}
