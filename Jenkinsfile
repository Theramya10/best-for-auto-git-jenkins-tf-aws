pipeline {
    agent any

    parameters {
        booleanParam(name: 'RUN_TERRAFORM_INIT', defaultValue: false, description: 'Run Terraform Init?')
        booleanParam(name: 'RUN_TERRAFORM_VALIDATE', defaultValue: false, description: 'Run Terraform Validate?')
        booleanParam(name: 'RUN_TERRAFORM_PLAN', defaultValue: false, description: 'Run Terraform Plan?')
        booleanParam(name: 'RUN_TERRAFORM_APPLY', defaultValue: false, description: 'Run Terraform Apply?')
        booleanParam(name: 'DESTROY_ENVIRONMENT', defaultValue: false, description: 'Destroy Terraform Infrastructure?')
    }

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')  
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')  
    }

    stages {

        stage('Terraform Init') {
            when {
                expression { params.RUN_TERRAFORM_INIT }
            }
            steps {
                script {
                    echo "Running Terraform Init..."
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            when {
                expression { params.RUN_TERRAFORM_VALIDATE }
            }
            steps {
                script {
                    echo "Running Terraform Validate..."
                    sh 'terraform validate'
                }
            }
        }

        stage('Terraform Plan') {
            when {
                expression { params.RUN_TERRAFORM_PLAN }
            }
            steps {
                script {
                    echo "Running Terraform Plan..."
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            when {
                expression { params.RUN_TERRAFORM_APPLY }
            }
            steps {
                script {
                    input message: "Do you want to proceed with Terraform Apply?", ok: "Yes"
                    echo "Applying Terraform Configuration..."
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.DESTROY_ENVIRONMENT }
            }
            steps {
                script {
                    input message: "Are you sure you want to destroy the infrastructure?", ok: "Destroy"
                    echo "Destroying Terraform Infrastructure..."
                    sh 'terraform destroy -auto-approve'
                }
            }
        }
    }
}
