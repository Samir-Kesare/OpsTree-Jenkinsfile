pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        TF_CLI_ARGS           = '-input=false'
    }
    
    parameters {
        choice(name: 'ACTION', choices: ['Apply', 'Destroy'], description: 'Choose to apply or destroy the infrastructure')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'Shantanu/Employee/Dev', credentialsId: 'shantanu-pat', url: 'https://github.com/CodeOps-Hub/Terraform-modules.git'
            }
        }
        
        stage('Copy Terraform Files') {
            steps {
                sh 'cp wrapperCode/Employee_Wrapper_Code/Dev_Env/* .'
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
        
        stage('Review and Approve Apply') {
            when {
                expression { params.ACTION == 'Apply' }
            }
            steps {
                input "Do you want to apply Terraform changes?"
            }
        }
        
        stage('Review and Approve Destroy') {
            when {
                expression { params.ACTION == 'Destroy' }
            }
            steps {
                input "Do you want to destroy Terraform resources?"
            }
        }
        
        stage('Apply or Destroy') {
            steps {
                script {
                    sh "terraform ${params.ACTION.toLowerCase()} -auto-approve"
                }
            }
        }
    }
    
    post {
        success {
            script {
                    echo 'Terraform operation successful!'
                    archiveArtifacts artifacts: '*.pem', allowEmptyArchive: true
            }
        }
        failure {
            echo 'Terraform operation failed!'
        }
    }
}
