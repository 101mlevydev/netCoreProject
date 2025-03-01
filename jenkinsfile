pipeline {
    agent any
    environment {
        K8S_NAMESPACE = "webapp"
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
  
