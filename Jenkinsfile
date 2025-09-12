pipeline {
    agent any

    environment {
        NODE_ENV = "production"
        FRONTEND_IMAGE = "prescripto-frontend:latest"
        BACKEND_IMAGE  = "prescripto-backend:latest"
    }

    options {
        timeout(time: 60, unit: 'MINUTES')
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/mafid456/prescripto_full-stack_doctor_appointment_app_Fork.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building frontend Docker image..."
                    sh "docker build -t ${env.FRONTEND_IMAGE} ./frontend"

                    echo "Building backend Docker image..."
                    sh "docker build -t ${env.BACKEND_IMAGE} ./backend"
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo "Stopping old containers..."
                sh 'docker-compose down || true'

                echo "Starting new containers..."
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
            echo "Pipeline failed!"
        }
    }
}
