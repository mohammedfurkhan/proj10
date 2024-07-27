pipeline {
    agent any
    environment {
        AZURE_CLIENT_ID = credentials('azure-client-id')
        AZURE_SECRET = credentials('azure-client-secret')
        AZURE_TENANT = credentials('azure-tenant-id')
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Login to Azure') {
            steps {
                bat """
                    az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_SECRET% --tenant %AZURE_TENANT%
                """
            }
        }
        stage('Terraform Init') {
            steps {
                dir('terraform') {
                    bat """
                        terraform init
                    """
                }
            }
        }
        stage('Terraform Plan') {
            steps {
                dir('terraform') {
                    bat """
                        terraform plan -out=tfplan
                    """
                }
            }
        }
        stage('Terraform Apply') {
            steps {
                dir('terraform') {
                    bat """
                        terraform apply -auto-approve tfplan
                    """
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn clean verify sonar:sonar'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
