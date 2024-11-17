pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Check Python') {
            steps {
                script {
                    sh "python --version || echo 'Python not found'"
                }
            }
        }
        stage('Setup') {
            steps {
                script {
                    if (isUnix()) {
                        sh "python -m venv ${VIRTUAL_ENV}"
                    } else {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh "set PYTHONPATH=${env.WORKSPACE} && source ${VIRTUAL_ENV}/bin/activate && pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    sh "set PYTHONPATH=${env.WORKSPACE} && source ${VIRTUAL_ENV}/bin/activate && coverage run -m pytest"
                    sh "source ${VIRTUAL_ENV}/bin/activate && coverage report"
                    sh "source ${VIRTUAL_ENV}/bin/activate && coverage html"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && bandit -r app/"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
