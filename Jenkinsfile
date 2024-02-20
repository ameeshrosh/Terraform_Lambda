
def profile

node {
  checkout scm
  def profilesConfig = readYaml(file: 'config/profiles.yaml')
  def currentProfile = (env.JOB_NAME =~ /[a-z]+$/).findAll().first()
  profile = profilesConfig.profiles[currentProfile]
}
pipeline {
  agent {
    docker {
      image 'hashicorp/terraform:latest'
      args '-v /var/run/docker.sock:/var/run/docker.sock --group-add docker'
      label 'linux-docker-node'
    }
  }
    
  options {
        disableConcurrentBuilds()
        buildDiscarder(
            logRotator(
                 numToKeepStr: '50',
                 artifactNumToKeepStr: '25'
      )
    )
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
    
