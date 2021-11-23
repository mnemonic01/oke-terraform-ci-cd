pipeline {

  agent any


  stages {

    stage('Checkout') {
      steps {
         sh 'PATH=/usr/local/bin'
         // sh 'terraform fmt'//
         sh 'terraform init' //only need for first run 
         // sh 'terraform refresh -lock=false'//
         // sh 'cp terraform.tfvars .'//
         sh 'ls'
               
      }
    }

    stage('TF Plan') {
      
    steps {
          
          sh 'PATH=/usr/local/bin'
         // sh 'terraform fmt'//
          sh 'terraform init' //only need for first run 
         // sh 'terraform refresh -lock=false'//
         sh 'cp vars.tf .'
          sh 'ls'
          sh 'terraform plan  -lock=false -out oke_plan'
      }      
    }

    stage('Approval') {
      steps {
        script {
          def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
        }
      }
    }

    stage('TF Apply') {
      steps {
          sh 'terraform apply -lock=false --auto-approve oke_plan'
        }
      }
    }
}
