pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {
        stage('Clone Terraform Repo') {
            steps {
                echo '🔍 Starting: Clone Terraform Repo'
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Yuga-23/terraform_assign.git',
                        credentialsId: 'github-credentials'
                    ]]
                ])
                echo '✅ Completed: Clone Terraform Repo'
            }
        }

        stage('Initialize Terraform') {
            steps {
                echo '🔧 Starting: Terraform Init'
                sh 'terraform init'
                echo '✅ Completed: Terraform Init'
            }
        }

        stage('Validate Terraform') {
            steps {
                echo '🔍 Starting: Terraform Validate'
                sh 'terraform validate'
                echo '✅ Completed: Terraform Validate'
            }
        }

        stage('Plan Infrastructure') {
            steps {
                echo '📐 Starting: Terraform Plan'
                sh 'terraform plan -out=tfplan'
                echo '✅ Completed: Terraform Plan'
            }
        }

        stage('Apply Infrastructure') {
            steps {
                echo '🚀 Ready to Apply: Awaiting Approval'
                input message: 'Approve Terraform Apply?'
                echo '🛠️ Starting: Terraform Apply'
                sh 'terraform apply tfplan'
                echo '✅ Completed: Terraform Apply'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline finished successfully!'
        }
        failure {
            echo '⚠️ Pipeline failed. Check which stage was last completed.'
        }
    }
}
