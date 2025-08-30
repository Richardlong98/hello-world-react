pipeline {
    agent any

    environment {
        APP_NAME = "hello-world-react"
        APP_PORT = "8888"
        REPO_URL = "https://github.com/aevitas/hello-world-react.git"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t ${APP_NAME}:latest .
                    """
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    # stop & remove container cũ (nếu tồn tại)
                    sh """
                    docker stop ${APP_NAME} || true
                    docker rm ${APP_NAME} || true

                    # chạy container mới
                    docker run -d -p ${APP_PORT}:80 --name ${APP_NAME} ${APP_NAME}:latest
                    """
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed!"
        }
        success {
            echo "✅ Deployment successful! App running on port ${APP_PORT}"
        }
    }
}
