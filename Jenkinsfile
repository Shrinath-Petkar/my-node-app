pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1.0.0', description: 'App Version')
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'qa', 'prod'], description: 'Deployment environment')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out repository..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing Node.js packages..."
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "Building Node.js project..."
                bat 'echo Build step (Node.js usually has no compile step)'
            }
        }

        stage('Test') {
            parallel {

                stage('Unit Tests') {
                    steps {
                        echo "Running unit tests..."
                        bat 'npm test'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        echo "Running integration tests..."
                        bat 'npm run integration-test'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Validating test results..."
            }
        }

        stage('Package') {
            steps {
                echo "Packaging application..."
                bat 'mkdir build'
                bat "powershell Compress-Archive -Path src\\* -DestinationPath build\\node-app-%VERSION%.zip -Force"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying version ${params.VERSION} to ${params.DEPLOY_ENV}..."
                bat 'echo Deployment simulated.'
            }
        }
    }

    post {
        success {
            echo "Archiving build artifacts..."
            archiveArtifacts artifacts: 'build*', fingerprint: true
        }

        failure {
            echo "Sending failure notification..."
        }

        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}
