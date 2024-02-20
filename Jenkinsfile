pipeline {
  agent {
    docker {
      image 'hashicorp/terraform:latest'
      args '-v /var/run/docker.sock:/var/run/docker.sock --group-add docker'
      label 'Built-In-Node'
    }
  }

    parameters {
            choice(name: 'action', choices: ['apply','destroy'], description: 'Terraform action')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
    
        stage ("terraform init") {
            steps {
                sh ("terraform init -reconfigure") 
            }
        }
        
        stage ("plan") {
            steps {
                sh ('terraform plan') 
            }
        }

        stage (" Action") {
            steps {
                echo "Terraform action is --> ${action}"
                sh ('terraform ${action} --auto-approve') 
           }
        }
    }
}
    
