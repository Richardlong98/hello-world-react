pipeline {
    agent any

    environment {
        APP_NAME = "hello-world-react"
        APP_PORT = "8888"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/aevitas/hello-world-react.git'
            }
        }

        stage('Build React App') {
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        docker build -t ${APP_NAME}:latest .
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                        # Stop container cũ nếu có
                        docker rm -f ${APP_NAME} || true
                        
                        # Run container mới
                        docker run -d --name ${APP_NAME} -p ${APP_PORT}:80 ${APP_NAME}:latest
                    '''
                }
            }
        }
    }
}
