pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "gourikulkarni/java-docker-app"
    }

    stages {
        stage('Build JAR') {
            steps {
                echo "🔧 Building Java application..."
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "🚀 Pushing image to Docker Hub..."
                bat """
                    echo %DOCKERHUB_CREDENTIALS_PSW% | docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin
                    docker tag %IMAGE_NAME%:latest %IMAGE_NAME%:%BUILD_NUMBER%
                    docker push %IMAGE_NAME%:latest
                    docker push %IMAGE_NAME%:%BUILD_NUMBER%
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build and push completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check the logs."
        }
    }
}


