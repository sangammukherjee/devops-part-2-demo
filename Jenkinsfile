pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = "mern-frontend:jenkins"
        BACKEND_IMAGE  = "mern-backend:jenkins"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/sangammukherjee/devops-part-2-demo.git', branch: 'master'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                  echo "Building backend image..."
                  docker build -t $BACKEND_IMAGE ./server

                  echo "Building frontend image..."
                  docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
                '''
            }
        }

        stage('Run with Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Test Services') {
            steps {
                sh '''
                  echo "Checking if backend is running..."
                  curl -f http://localhost:5000 || exit 1

                  echo "Checking if frontend is running..."
                  curl -f http://localhost:5173 || exit 1
                '''
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker compose down'
            }
        }
    }
}
