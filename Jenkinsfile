pipeline {
    agent any

    stages {
        stage('Clone Git Repo') {
            steps {
                git branch: 'main', credentialsId: '60017995-e2e3-439e-9dbd-7fda283c0015', url: 'https://github.com/mohammedfurkhan/proj10.git'
            }
        }
        stage('Version') {
            steps {
                sh ''' 
                terraform version
                    az --version              
                '''
            }
        }
        stage('Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Init - ugrade') {
            steps {
                sh 'terraform init -upgrade'
            }
        }
        stage('Clean up ') {
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }
        stage('validate') {
            steps {
                sh 'terraform validate'
            }
        }
        stage('Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }
}
