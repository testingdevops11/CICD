pipeline {

    parameters {
        string(name: 'environment', defaultValue: 'terraform', description: 'Workspace/environment file to use for deployment')
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
        string(name: 'Action', defaultValue: 'apply', description: 'Action/Apply create or destroy resources')

    }


     

   agent  any
        options {
                timestamps ()
               
            }
    stages {
        stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git "https://github.com/testingdevops11/terraform-ec2.git"
                        }
                    }
                }
            }

        stage('Plan') {
            steps {
                sh 'pwd;cd terraform/aws-instance-first-script ; terraform init -input=false'
               /* sh 'pwd;cd terraform/aws-instance-first-script ; terraform workspace new ${environment}'
                sh 'pwd;cd terraform/aws-instance-first-script ; terraform workspace select ${environment}'
                sh "pwd;cd terraform/aws-instance-first-script ;terraform plan -input=false -out tfplan "
                sh 'pwd;cd terraform/aws-instance-first-script ;terraform show -no-color tfplan > tfplan.txt'*/
            }
        }
       stage('Approval') {
           when {
               not {
                   equals expected: true, actual: params.autoApprove
               }
           }

           steps {
               script {
                    def plan = readFile 'terraform/aws-instance-first-script/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
               }
           }
       } 

        stage('Apply') {
            steps {
                sh "pwd;cd terraform/aws-instance-first-script ; terraform apply -input=false tfplan"
            }
        }
    }

  }
