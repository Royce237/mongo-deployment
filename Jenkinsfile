pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/Royce237/mongo-deployment.git'
        NAMESPACE_STAGE = 'stage'
        NAMESPACE_DEV = 'dev'
        NAMESPACE_PROD = 'prod'
        K8S_CLUSTER = 'default' // Kube context for accessing the cluster

    }

    stages {
        stage('Clone Git Repository') {
            steps {
                // Checkout the Git repository
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Deploy to Stage') {
            // Ensure deployment happens on the right agent
            steps {
                script {
                    // Helm install/upgrade in stage namespace
                    sh """
                        helm upgrade --install backend backend --namespace ${NAMESPACE_STAGE} --create-namespace
                        helm upgrade --install frontend frontend --namespace ${NAMESPACE_STAGE} --create-namespace
                    """
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                script {
                    // Helm install/upgrade in dev namespace
                    sh """
                        helm upgrade --install backend backend --namespace ${NAMESPACE_DEV} --create-namespace
                        helm upgrade --install frontend frontend --namespace ${NAMESPACE_DEV} --create-namespace
                    """
                }
            }
        }

        stage('Approve for Prod Deployment') {
            when {
                branch 'main' // Prod deployment only happens from the main branch
            }
            steps {
                // User input required for prod deployment
                input message: 'Do you want to deploy to production?', ok: 'Deploy'
            }
        }

        stage('Deploy to Prod'{
            when {
                branch 'main'
            }
            steps {
                script {
                    // Helm install/upgrade in prod namespace
                    sh """
                        helm upgrade --install backend backend --namespace ${NAMESPACE_PROD} --create-namespace
                        helm upgrade --install frontend frontend --namespace ${NAMESPACE_PROD} --create-namespace
                    """
                }
            }
        }
    }

    post {
        always {
            // Notifications or cleanup if necessary
            echo "Deployment pipeline completed."
        }
    }
}
