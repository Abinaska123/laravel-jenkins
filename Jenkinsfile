pipeline {
    agent any

    environment {
        DOCKER_REGISTRY_URL = 'your-docker-registry'
        DOCKER_IMAGE_NAME = 'your-laravel-app'
        DOCKER_IMAGE_TAG = 'latest'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        DOCKERFILE_PATH = 'docker/php/Dockerfile'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Docker Compose') {
            steps {
                script {
                    sh 'sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                    sh 'sudo chmod +x /usr/local/bin/docker-compose'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Laravel application
                    sh "docker-compose -f $DOCKER_COMPOSE_FILE -f $DOCKERFILE_PATH build"
                }
            }
        }

        stage('Tag and Push Docker Image') {
            steps {
                script {
                    // Tag Docker image
                    sh "docker tag $DOCKER_IMAGE_NAME $DOCKER_REGISTRY_URL/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
                    
                    // Push Docker image to registry
                    sh "docker push $DOCKER_REGISTRY_URL/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
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
                    sh 'docker-compose -f $DOCKER_COMPOSE_FILE exec laravel-app php artisan test'
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

