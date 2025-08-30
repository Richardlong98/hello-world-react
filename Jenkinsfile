pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hello-world-react"
        NODE_OPTIONS = "--openssl-legacy-provider"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/aevitas/hello-world-react.git'
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                    echo "===== Cài dependencies ====="
                    npm install
                    echo "===== Build React App ====="
                    npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "===== Build Docker Image ====="
                    docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    echo "===== Stop container cũ nếu có ====="
                    docker stop $DOCKER_IMAGE || true
                    docker rm $DOCKER_IMAGE || true

                    echo "===== Run container mới ====="
                    docker run -d --name $DOCKER_IMAGE -p 8888:80 $DOCKER_IMAGE
                '''
            }
        }
    }
}
