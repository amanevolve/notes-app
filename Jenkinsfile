pipeline {
    agent any

    environment {
        IMAGE_NAME = "notes-app:latest"
        CONTAINER_NAME = "notes-app-container"
        PORT = "9092"
    }

    stages {

        stage('checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/amanevolve/notes-app.git'
            }
        }

        stage('Build Docker Inages') {
            steps {
                sh '''
                echo "=======Building Docker Images======="
                docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                echo "=======Stopping OLD Conatiner======="
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Start New Container') {
            steps {
                sh '''
                echo "=======Starting New Container======="
                docker run -d --name ${CONTAINER_NAME} -p $PORT:80 -v notes-data:/data ${IMAGE_NAME}
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                echo "=======Checking app response======="
                sleep 5
                curl -s http://54.224.112.212:${PORT} | head -n 20
                '''
            }
        }
    }

    post {
        success {
            echo "Notes App deployed successfully: http://http://54.175.118.235:9092/"
        }
        failure {
            echo "Notes APP Deployment Failed. Check logs"
        }
    }
}
