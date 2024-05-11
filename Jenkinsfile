pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'subh12/todoapp:latest'
        APP_PORT = '5000'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using the provided Dockerfile
                    docker.build("${DOCKER_IMAGE}", "--file Dockerfile .")
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run tests (if applicable) inside the Docker container
                    docker.image("${DOCKER_IMAGE}").inside("--publish ${APP_PORT}:${APP_PORT}") {
                        sh 'pip3 install --no-cache-dir -r requirements.txt'
                        sh 'python3 -m pytest'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Deploy application by running the Docker container
                    docker.image("${DOCKER_IMAGE}").run("--publish ${APP_PORT}:${APP_PORT}")
                }
            }
        }
    }
    
    post {
        always {
            // Clean up workspace after execution
            cleanWs()
        }
    }
}
