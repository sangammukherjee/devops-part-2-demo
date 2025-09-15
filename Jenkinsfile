pipeline {
  agent any

  environment {
    FRONTEND_IMAGE = "mern-frontend:jenkins"
    BACKEND_IMAGE  = "mern-backend:jenkins"
    PORT = "5000"
    MONGO_URI = "mongodb://mongo:27017/taskdb"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/sangammukherjee/devops-part-2-demo.git', branch: 'master'
      }
    }

    stage('Prepare .env') {
      steps {
        sh '''
          mkdir -p server
          cat > server/.env <<EOF
PORT=$PORT
MONGO_URI=$MONGO_URI
EOF
        '''
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
        sh '''
          echo "Starting MERN stack with Docker Compose..."
          docker compose up -d

          echo "Showing running containers..."
          docker ps

          echo "===== Backend Logs ====="
          docker logs backend || true

          echo "===== Frontend Logs ====="
          docker logs frontend || true
        '''
      }
    }
  }
}
