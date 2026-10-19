pipeline {
    agent any

    environment {
        GIT_REPOSITORY_URL = 'https://github.com/sudhamshcdac/docker_jenkins_demo.git'
        DOCKER_IMAGE_NAME  = 'sudhamshcdac/docker_jenkins_demo'
        IMAGE_TAG          = '1.0'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        git branch: 'main', url: "${env.GIT_REPOSITORY_URL}"
                    } catch (Exception e) {
                        echo "Failed to clone repository: ${e.message}"
                        error "Failed to clone repository"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        // Use string interpolation properly for Docker build context
                        docker.build("${env.DOCKER_IMAGE_NAME}:${env.IMAGE_TAG}", ".")
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        error "Failed to build Docker image"
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', 
                                                         usernameVariable: 'DOCKER_USERNAME', 
                                                         passwordVariable: 'DOCKER_PASSWORD')]) {
                            // Native Jenkins Docker Pipeline plugin syntax instead of shell scripting
                            docker.withRegistry('https://docker.io', 'my-docker-hub-credentials-id') {
                                docker.image("${env.DOCKER_IMAGE_NAME}:${env.IMAGE_TAG}").push()
                            }
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker image to registry: ${e.message}"
                        error "Failed to push Docker image"
                    }
                }
            }
        }
    }
}

