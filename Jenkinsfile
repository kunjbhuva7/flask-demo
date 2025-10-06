pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'kunj22'
        IMAGE_NAME = 'flask-demo'
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
        RECIPIENT_EMAIL = 'kunjbhuva301@gmail.com' //
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
                script {
                    if (env.DOCKER_PASS) {
                        echo "Simulating Docker login and push..."
                        echo "docker login -u $DOCKERHUB_USER --password-stdin <<< $DOCKER_PASS"
                        echo "docker push $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                    } else {
                        echo "Skipping Docker push simulation (credential not found)"
                    }
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
            
            // HTML email on success
            emailext(
                subject: "🎉 Jenkins Pipeline Success: Build #${BUILD_NUMBER}",
                to: "kunjbhuva301@gmail.com",
                mimeType: 'text/html',
                body: """
                <html>
                <head>
                    <style>
                        body { font-family: Arial, sans-serif; background-color: #f0f8ff; color: #333; }
                        .container { padding: 20px; border-radius: 10px; background-color: #e6f7ff; border: 2px solid #00aaff; }
                        h1 { color: #0077cc; }
                        p { font-size: 16px; }
                        .success { color: green; font-weight: bold; }
                        .footer { margin-top: 20px; font-size: 12px; color: #555; }
                        .button {
                            display: inline-block;
                            padding: 10px 20px;
                            font-size: 16px;
                            color: white;
                            background-color: #00aaff;
                            border-radius: 5px;
                            text-decoration: none;
                        }
                    </style>
                </head>
                <body>
                    <div class="container">
                        <h1>Jenkins Pipeline Success ✅</h1>
                        <p>Pipeline for project <strong>${IMAGE_NAME}</strong> executed successfully.</p>
                        <p>Build Number: <strong>${BUILD_NUMBER}</strong></p>
                        <p class="success">All stages completed without errors!</p>
                        <a class="button" href="${BUILD_URL}">View Build Details</a>
                        <div class="footer">This is an automated message from Jenkins.</div>
                    </div>
                </body>
                </html>
                """
            )
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}

