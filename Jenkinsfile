pipeline {
    agent any

    environment {
        // Corrected username typo from sudhamshdcac to sudhamshcdac to match your GitHub repository
        GIT_REPOSITORY_URL = 'https://github.com/sudhamshcdac/docker_jenkins_demo.git'
        DOCKER_IMAGE_NAME  = 'sudhamshcdac/docker_jenkins_demo'
        IMAGE_TAG          = '1.0'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        // Using explicit env scope for script string execution
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
                        // Added build context root path "." to prevent pipeline workspace resolution bugs
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
                        // Replaced risky manual 'sh' logins with the native, secure Jenkins Docker plugin wrapper
                        docker.withRegistry('https://docker.io', 'my-docker-hub-credentials-id') {
                            docker.image("${env.DOCKER_IMAGE_NAME}:${env.IMAGE_TAG}").push()
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

