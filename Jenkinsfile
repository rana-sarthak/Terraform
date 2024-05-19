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
            terraform init
            terraform plan -out=tfplan
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
              terraform destroy -auto-approve
              '''
            }
          }
        }

      }
    }
