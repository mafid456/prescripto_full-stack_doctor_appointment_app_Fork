pipeline {
    agent any

    environment {
        // Set any environment variables if needed
        NODE_ENV = "production"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/mafid456/prescripto_full-stack_doctor_appointment_app_Fork.git'
            }
        }

        stage('Install Dependencies') {
            parallel {
                stage('Frontend Dependencies') {
                    steps {
                        dir('frontend') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Backend Dependencies') {
                    steps {
                        dir('backend') {
                            sh 'npm install'
                        }
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building Docker images..."
                    sh 'docker build -t prescripto-frontend:latest ./frontend'
                    sh 'docker build -t prescripto-backend:latest ./backend'
                }
            }
        }

        stage('Run Containers') {
            steps {
                echo "Starting containers..."
                // If you have docker-compose.yml
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
            }
        }

        stage('Post Deployment') {
            steps {
                echo "Deployment completed successfully!"
                sh 'docker ps'
            }
        }
    }

    post {
        always {
            echo "Cleaning up unused Docker resources..."
            sh 'docker system prune -f'
        }
        success {
            echo "Pipeline finished successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
