pipeline {

    agent any

    environment {

        AWS_REGION = "ap-southeast-2"

    }

    stages {

        stage('Checkout') {

            steps {

                git branch: 'main',

                url: 'https://github.com/your-org/terraform-ec2.git'

            }

        }

        stage('Terraform Init') {

            steps {

                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-prod'
                ]]) {

                    sh 'terraform init'

                }

            }

        }

        stage('Terraform Format') {

            steps {

                sh 'terraform fmt'

            }

        }

        stage('Terraform Validate') {

            steps {

                sh 'terraform validate'

            }

        }

        stage('Terraform Plan') {

            steps {

                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-prod'
                ]]) {

                    sh 'terraform plan'

                }

            }

        }

        stage('Approval') {

            steps {

                input "Deploy Infrastructure?"

            }

        }

        stage('Terraform Apply') {

            steps {

                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-prod'
                ]]) {

                    sh 'terraform apply -auto-approve'

                }

            }

        }

    }

}
