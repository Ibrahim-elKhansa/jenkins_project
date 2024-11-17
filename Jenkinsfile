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
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        if (isUnix()) {
                            sh "python -m venv ${VIRTUAL_ENV}"
                        } else {
                            bat "python -m venv ${VIRTUAL_ENV}"
                        }
                    }
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                    }
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                    }
                }
            }
        }
        stage('Test') {
        steps {
            script {
                if (isUnix()) {
                    sh "export PYTHONPATH=${env.WORKSPACE} && source ${VIRTUAL_ENV}/bin/activate && pytest"
                } else {
                    bat "set PYTHONPATH=${env.WORKSPACE} && ${VIRTUAL_ENV}\\Scripts\\activate && pytest"
                }
            }
        }
    }
        stage('Deploy') {
            steps {
                script {
                    // Deployment logic
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
