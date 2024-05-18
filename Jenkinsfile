pipeline {
  agent any
  parameters {
    choice(name: 'ACTION', choices: ['Apply', 'Destroy'], description: 'Select the action')
  }

environment {
        AWS_CREDENTIALS = credentials('IAM User')
      }

  stages {
    stage('Clone Repository') {
      steps {
        git branch: 'main', url: 'https://github.com/rana-sarthak/Terraform.git'
      }
    }
    stage('Terraform Apply') {
      when {
        expression {
          params.ACTION == 'Apply'
        }
      }
        steps {
          script {
            bat '''
            dir
	    echo 'Running Terraform Apply'
            terraform init
            terraform plan
	    terraform apply 
            '''
          }
        }
      }
      stage('Terraform Destroy') {
        when {
          expression {
            params.ACTION == 'Destroy'
          }
        }
          steps {
            script {
              echo 'Running Terraform Apply'
              bat '''
	      dir
              terraform destroy
              '''
            }
          }
        }
      }
    }