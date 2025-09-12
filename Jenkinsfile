pipeline {
    agent any

    environment {
        NODE_ENV = "production"
        FRONTEND_IMAGE = "prescripto-frontend:latest"
        BACKEND_IMAGE  = "prescripto-backend:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/mafid456/prescripto_full-stack_doctor_appointment_app_Fork.git'
            }
        }

        stage('Install Dependencies & Build Frontend') {
            steps {
                dir('frontend') {
                    echo "Building frontend inside Node.js container..."
                    sh '''
                        docker run --rm -v $PWD:/app -w /app node:18 bash -c "npm install && npm run build"
                    '''
                }
            }
        }

        stage('Install Dependencies & Build Backend') {
            steps {
                dir('backend') {
                    echo "Building backend inside Node.js container..."
                    sh '''
                        docker run --rm -v $PWD:/app -w /app node:18 bash -c "npm install"
                    '''
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building Docker images..."
                    sh 'docker build -t $FRONTEND_IMAGE ./frontend'
                    sh 'docker build -t $BACKEND_IMAGE ./backend'
                }
            }
        }

        stage('Run Containers') {
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
