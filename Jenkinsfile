pipeline {
    agent any
    environment {
        AZURE_CLIENT_ID = credentials('azure-client-id-tenant')
        AZURE_CLIENT_SECRET = credentials('azure-client-secret')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mohammedfurkhan/proj10.git'
            }
        }
        stage('Login to Azure') {
            steps {
                sh '''
                az login --service-principal -u $AZURE_CLIENT_ID_USR -p $AZURE_CLIENT_SECRET --tenant $AZURE_CLIENT_ID_PSW
                '''
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                    sh 'sonar-scanner'
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
