pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent  any
    stages {
        stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git "https://github.com/Kaustubh9804/Tool_MySQL.git"
                        }
                    }
                }
            }

        stage('Plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform init'
                sh 'pwd;cd terraform/ ; terraform validate'
                sh 'pwd;cd terraform/ ; terraform plan'
            }
        }
            
        stage('Apply') {
            steps {
                sh "pwd;cd terraform/ ; terraform apply -auto-approve"
            }
        }
    }

}
