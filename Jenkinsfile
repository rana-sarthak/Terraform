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
            terraform init
            terraform plan
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
              echo 'Hello World'
              bat '''
              terraform --version
              dir
              '''
            }
          }
        }
      }
    }