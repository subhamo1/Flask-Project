// Jenkinsfile (Declarative Pipeline)

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/subhamo1/Flask-Project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'python -m unittest discover tests/unit'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh 'python -m unittest discover tests/integration'
                    }
                }
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'main'
            }
            steps {
                sh 'scp -r . staging_user@staging_host:/path/to/staging/app'
                sh 'ssh staging_user@staging_host "/path/to/staging/venv/bin/pip install -r /path/to/staging/app/requirements.txt"'
                sh 'ssh staging_user@staging_host "/path/to/staging/venv/bin/python /path/to/staging/app/app.py"'
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input "Deploy to Production?"
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                sh 'scp -r . prod_user@prod_host:/path/to/prod/app'
                sh 'ssh prod_user@prod_host "/path/to/prod/venv/bin/pip install -r /path/to/prod/app/requirements.txt"'
                sh 'ssh prod_user@prod_host "/path/to/prod/venv/bin/python /path/to/prod/app/app.py"'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
