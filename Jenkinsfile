pipeline {

  agent any

  environment {
    
    PEM_PRIVATE_KEY = credentials('private_key')
    OCI_OCID_VAR = credentials('oci_vars')
    
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
        sh 'mkdir -p creds' 
        sh 'ls'
        sh 'echo $PEM_PRIVATE_KEY'
        sh 'echo $PEM_PRIVATE_KEY | base64 -d > ./creds/oci_api_key.pem'
        sh 'echo $OCI_OCID_VAR | base64 -d > vars.tf'
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
