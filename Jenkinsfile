pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'kunj22'
        IMAGE_NAME = 'flask-demo'
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kunjbhuva7/flask-demo.git'
            }
        }

        stage('Build Docker Image (Simulated)') {
            steps {
                sh '''
                echo "----------------------------------------"
                echo "Simulating Docker build..."
                echo "docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER ."
                echo "----------------------------------------"
                '''
            }
        }

        stage('Scan with Trivy (Simulated)') {
            steps {
                sh '''
                echo "----------------------------------------"
                echo "Simulating Trivy scan on $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                echo "trivy image $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                echo "----------------------------------------"
                '''
            }
        }

        stage('Push to DockerHub (Simulated)') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh '''
                    echo "----------------------------------------"
                    echo "Simulating Docker login..."
                    echo "docker login -u $DOCKERHUB_USER --password-stdin"
                    echo "Simulating docker push $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                    echo "Simulating docker tag $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER $DOCKERHUB_USER/$IMAGE_NAME:latest"
                    echo "Simulating docker push latest"
                    echo "----------------------------------------"
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes (Simulated)') {
            steps {
                sh '''
                echo "----------------------------------------"
                echo "Simulating kubectl apply -f deployment.yaml"
                echo "----------------------------------------"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully (simulated)!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}

