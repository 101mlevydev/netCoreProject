pipeline {
    agent any

    environment {
        K8S_NAMESPACE_DEV = "devops"
        K8S_NAMESPACE_PROD = "production"
        APP_NAME = "netcoreapp"
        IMAGE_NAME = "netcoreapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/101mlevydev/netCoreProject'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Load Image to Kind') {
            steps {
                sh 'kind load docker-image $IMAGE_NAME'
            }
        }

        stage('Deploy to Production Namespace') {
            steps {
                script {
                    sh "kubectl create namespace ${K8S_NAMESPACE_PROD} || true"
                    sh "kubectl apply -f deployment.yaml --namespace=${K8S_NAMESPACE_PROD}"
                }
            }
        }
    }
}
