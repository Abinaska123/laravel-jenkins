pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'your-docker-registry'
        IMAGE_NAME = 'your-laravel-app'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        DOCKERFILE_PATH = 'docker/php/Dockerfile'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Laravel application
                    sh 'docker-compose -f $DOCKER_COMPOSE_FILE build'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker image to registry
                    sh "docker-compose -f $DOCKER_COMPOSE_FILE push"
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

