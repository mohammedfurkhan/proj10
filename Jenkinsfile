pipeline {
    agent any
    environment {
        AZURE_TENANT_ID = credentials('azure-tenant-id')
        AZURE_CLIENT_ID = credentials('azure-client-id-tenant')
        AZURE_CLIENT_SECRET = credentials('azure-client-secret')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Login to Azure') {
            steps {
                script {
                    bat '''
                    az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%
                    '''
                }
            }
        }
        stage('Terraform Init') {
            steps {
                script {
                    bat 'terraform init'
                }
            }
        }
        stage('Terraform Plan') {
            steps {
                script {
                    bat 'terraform plan'
                }
            }
        }
        stage('Terraform Apply') {
            steps {
                script {
                    bat 'terraform apply -auto-approve'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    bat 'sonar-scanner'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
