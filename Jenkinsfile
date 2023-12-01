pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-north-1'  // Replace with your AWS region
        AWS_ACCOUNT_ID = '591481069844'     // Replace with your AWS account ID
        DOCKER_REGISTRY_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
        DOCKER_IMAGE_NAME = "docker-laravel-laravel-app"
        DOCKER_IMAGE_TAG = "latest"
        //DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        //DOCKERFILE_PATH = 'docker/php/Dockerfile'
        DOCKER_IMAGE_NAMENGNIX = "docker-laravel-nginx"
    }

    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Docker Compose') {
            steps {
                script {
                    sh 'sudo curl -fsSL https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose'
                    sh 'sudo chmod +x /usr/local/bin/docker-compose'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Laravel application   
                    //docker-compose build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                    sh "docker-compose build"
                    
                }
            }
        }

        stage('Tag and Push Docker Image') {
            steps {
                script {
                    //sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                    //sh "docker push ${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${DOCKER_REGISTRY_URL}:$DOCKER_IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                sh "docker tag ${DOCKER_IMAGE_NAMENGNIX}:${DOCKER_IMAGE_TAG} ${DOCKER_REGISTRY_URL}:$DOCKER_IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${DOCKER_IMAGE_NAMENGNIX}:${DOCKER_IMAGE_TAG}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy your application using Docker Compose
                    sh "docker-compose -f $DOCKER_COMPOSE_FILE up -d"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run Laravel tests
                    sh "docker-compose -f $DOCKER_COMPOSE_FILE exec laravel-app php artisan test"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
