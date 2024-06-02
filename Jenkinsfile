pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        APP_DIR = 'flask_app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    . $VENV_DIR/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    pytest
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    python setup.py install
                '''
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh '''
                    echo "Deploying to Staging environment"
                    # Add your deployment commands here, e.g., copying files to a server
                '''
            }
        }

        stage('Approval for Production Deployment') {
            steps {
                input 'Approve deployment to production?'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh '''
                    echo "Deploying to Production environment"
                    # Add your production deployment commands here
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
