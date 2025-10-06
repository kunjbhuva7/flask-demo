pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'kunj22'
        IMAGE_NAME = 'flask-demo'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kunjbhuva7/flask-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                # Run DinD container in background
                docker run --rm --privileged --name dind-temp -d docker:20.10.24-dind
                echo "Waiting for Docker daemon to start..."
                sleep 10

                # Copy current code to DinD container
                docker cp . dind-temp:/app

                # Build Docker image inside DinD container
                docker exec dind-temp docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER /app

                # Stop DinD container
                docker stop dind-temp || true
                '''
            }
        }

        stage('Scan with Trivy') {
            steps {
                sh '''
                # Optional: Run Trivy inside a container
                docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER || true
                '''
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKERHUB_USER --password-stdin
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER
                    docker tag $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER $DOCKERHUB_USER/$IMAGE_NAME:latest
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}

