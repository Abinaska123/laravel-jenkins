pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-north-1'
        AWS_ACCOUNT_ID = '591481069844'
    }

    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
            }
        }
    }
