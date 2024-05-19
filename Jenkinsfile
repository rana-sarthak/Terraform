pipeline {
  agent any
  parameters {
    choice(name: 'ACTION', choices: ['Apply', 'Destroy'], description: 'Select the action')
    booleanParam(name: 'Apply', defaultValue: false, description: 'True will apply the plan, default is false')
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
    stage('Terraform Apply Plan') {
      when {
        expression {
          params.ACTION == 'Apply'
        }
      }
        steps {
          script {
            bat '''
            dir
	        echo 'Running Terraform Apply Plan'
	        move /Y main.tf "X:\\Terraform Workspace"
	        cd /d X:\\Terraform Workspace
	        terraform plan -out=tfplan
            dir
            '''
          }
        }
      }
stage('Terraform Apply') {
      when {
        expression {
          return (params.ACTION == 'Apply' && params.Apply)
        }
      }
        steps {
          script {
            bat '''
            dir
	        echo 'Running Terraform Apply'
	        cd /d X:\\Terraform Workspace
	        terraform apply -auto-approve tfplan
            '''
          }
        }
      }
      stage('Terraform Destroy plan') {
        when {
          expression {
            params.ACTION == 'Destroy'
          }
        }
          steps {
            script {
              echo 'Running Terraform Destroy plan'
              bat '''
              move /Y main.tf "X:\\Terraform Workspace"
	          cd /d X:\\Terraform Workspace
	          dir
              terraform plan -destroy
              '''
            }
          }
        }
stage('Terraform Destroy') {
        when {
          expression {
            return (params.ACTION == 'Destroy' && params.Apply)
          }
        }
          steps {
            script {
              echo 'Running Terraform Destroy'
              bat '''
	          dir
	          cd /d X:\\Terraform Workspace
              terraform destroy -auto-approve
              '''
            }
          }
        }

      }
    }
