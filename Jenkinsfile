pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Kéo mã nguồn từ kho lưu trữ Git
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Cấu hình môi trường để sử dụng Docker daemon của Minikube
                    sh 'eval $(minikube -p minikube docker-env)'
                    
                    // Xây dựng image Docker với tag là số build của Jenkins
                    sh 'docker build -t spring-boot-app:latest.'
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                // Áp dụng tệp manifest để triển khai hoặc cập nhật ứng dụng
                sh 'kubectl apply -f k8s/deployment.yaml'
                
                // Chờ đợi rollout hoàn tất và hiển thị trạng thái pods
                sh 'kubectl rollout status deployment/spring-boot-app'
                sh 'kubectl get pods'
            }
        }
    }
}