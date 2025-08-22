pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Yuga-23/terraform_assign.git', credentialsId: 'github-credentials'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'PATH=/usr/local/bin:$PATH terraform init -upgrade'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'PATH=/usr/local/bin:$PATH terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'PATH=/usr/local/bin:$PATH terraform plan -out=tfplan'
                echo ' Plan generated.'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'PATH=/usr/local/bin:$PATH terraform apply -auto-approve tfplan'
                echo ' Apply complete.'
            }
        }
    }


}
