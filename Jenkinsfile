pipeline {
    agent any
    tools {
        terraform 'terraform'
        }
    parameters {
  choice choices: ['apply', 'destroy'], name: 'terraform_state'
}
    stages {
     stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/sansukh/terraform-high_availabilty-jenkins.git'
            }
        } 
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
            
        }
        stage('Terraform Plan') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
                {
                sh 'terraform plan'
                }
            }
            }
        stage('Terraform apply/destroy') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
                {
                    sh 'terraform ${terraform_state} --auto-approve'
                }
            }
        }
    }
}