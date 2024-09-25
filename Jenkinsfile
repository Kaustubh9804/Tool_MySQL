pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Select the action to perform')
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
                sh 'pwd;cd Terraform/ ; terraform plan -out tfplan'
                sh 'pwd;cd Terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }
      
        stage('Apply / Destroy') {
            steps {
                script {
                    if (params.action == 'apply') {
                        if (!params.autoApprove) {
                            def plan = readFile 'tfplan.txt'
                            input message: "Do you want to apply the plan?",
                            parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                        }

                        sh 'pwd;cd Terraform ; terraform ${action} -input=false tfplan'
                    } else if (params.action == 'destroy') {
                        sh 'pwd;cd Terraform ; terraform ${action} --auto-approve'
                    } else {
                        error "Invalid action selected. Please choose either 'apply' or 'destroy'."
                    }
                }
            }
        }        
        
    }

}
