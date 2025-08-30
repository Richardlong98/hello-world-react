pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        docker build -t hello-world-react .
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                        # Dừng container cũ nếu có
                        docker stop hello-world-react || true
                        docker rm hello-world-react || true

                        # Chạy container mới
                        docker run -d -p 8888:80 --name hello-world-react hello-world-react
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed!"
        }
        success {
            echo "✅ Build & deploy thành công!"
        }
    }
}
