pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {
        stage('Debug Git') {
            steps {
                sh '''
                    echo "Checking Git installation..."
                    which git
                    git --version
                    echo "Current user:"
                    whoami
                    echo "Environment PATH:"
                    echo $PATH
                '''
            }
        }

        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/Yuga-23/terraform_assign.git', branch: 'main'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }
}
