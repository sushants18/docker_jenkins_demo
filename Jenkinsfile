pipeline {
    agent any
    
    environment {
        // Define environment variables for Git and DockerHub credentials
        GIT_REPOSITORY_URL = 'https://github.com/newdelthis/docker_jenkins_demo.git'
        DOCKER_IMAGE_NAME = 'newdelthis/docker_jenkins_demo'
        IMAGE_TAG = '1.0' // Make sure to use string for tag
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the Git repository
                script {
                    try {
                        git branch: 'main', url: GIT_REPOSITORY_URL
                    } catch (Exception e) {
                        echo "Failed to clone repository: ${e.message}"
                        error "Failed to clone repository"
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build Docker image using Dockerfile in repository
                script {
                    try {
                        // Explicitly tag the image during build
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        error "Failed to build Docker image"
                    }
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                // Log in to DockerHub and push Docker image
                script {
                    try {
                        //withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
			docker.withRegistry('https://registry-1.docker.io/v2/', 'my-docker-hub-credentials-id') {
                            // Add debug output
                            echo "Docker Username: $DOCKER_USERNAME"
                            echo "Docker Image Name: $DOCKER_IMAGE_NAME"
                            
                            // Push the Docker image
                            docker.withRegistry('https://docker.io') {
                                docker.image("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}").push()
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
