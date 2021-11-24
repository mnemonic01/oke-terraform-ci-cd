pipeline {

  agent any


  stages {

    stage('Checkout') {
      steps {
         sh 'PATH=/usr/local/bin'
         // sh 'terraform fmt'//
         println 'Initiate Terraform provider'
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
         //sh 'cp vars.tf .'//
          println 'List all files needed'
          sh 'ls'
          println 'Terraform plan oke'
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
          println 'Apply the oke_plan'
          sh 'terraform apply -lock=false --auto-approve oke_plan'
        }
      }
    }
}
