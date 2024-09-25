pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = 'ap-south-1'
    }

   agent  any
        stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Kaustubh9804/Tool_MySQL.git', branch: 'main'
            }
        }

        stage('Plan') {
            steps {
                sh 'pwd;cd Terraform/ ; terraform init'
                sh 'pwd;cd Terraform/ ; terraform validate'
                sh 'pwd;cd Terraform/ ; terraform plan'
            }
        }

        stage('Approval') {
           when {
               not {
                   equals expected: true, actual: params.autoApprove
               }
           }
            
        stage('Apply') {
            steps {
                sh 'pwd;cd Terraform/ ; terraform apply -auto-approve'
            }
        }
        
         stage('Destroy') {
            steps {
                sh 'pwd;cd Terraform/ ; terraform destroy -auto-approve'
            }
        }
        
    }

}
