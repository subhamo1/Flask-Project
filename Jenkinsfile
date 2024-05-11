pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'subh12/todoapp:latest'
        APP_PORT = '5000'  // Use port 5000 for the Flask application
    }
    
    stages {
        stage('Pull Docker Image') {
            steps {
                // Pull the Docker image from Docker Hub
                script {
                    docker.image("${DOCKER_IMAGE}").pull()
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run tests inside the Docker container
                script {
                    docker.image("${DOCKER_IMAGE}").inside("--publish ${APP_PORT}:${APP_PORT}") {
                        // Install application dependencies and run tests
                        sh 'pip3 install --no-cache-dir -r requirements.txt'
                        sh 'python3 -m pytest'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // Example: Deploy the application (start Docker container)
                script {
                    docker.image("${DOCKER_IMAGE}").run("--publish ${APP_PORT}:${APP_PORT}")
                }
            }
        }
    }
    
    post {
        always {
            // Clean up Docker images after execution
            cleanWs()
        }
    }
}
