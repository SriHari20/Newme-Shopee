pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        DOCKER_IMAGE = "yourdockerhubusername/ekart-backend"
        KUBECONFIG = '/home/jenkins/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SriHari20/Newme-Shopee.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE).push('latest')
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f Newme-Shopee/k8s/ekart-secrets.yaml
                kubectl apply -f Newme-Shopee/k8s/db-deployment.yaml
                kubectl apply -f Newme-Shopee/k8s/redis-deployment.yaml
                kubectl apply -f Newme-Shopee/k8s/app-deployment.yaml
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
