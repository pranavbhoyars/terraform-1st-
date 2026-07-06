pipeline {

    agent {
        label 'my-agent'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/pranavbhoyars/terraform-1st-.git'
            }
        }

        stage('Terraform Version') {
            steps {
                sh 'terraform version'
                sh 'aws --version'
            }
        }

        stage('AWS Authentication') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt -check'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Approval') {
            steps {
                input message: 'Do you want to create AWS Infrastructure?'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {

        success {
            echo 'Terraform Infrastructure Created Successfully'
        }

        failure {
            echo 'Terraform Pipeline Failed'
        }

        always {
            cleanWs()
        }
    }
}
