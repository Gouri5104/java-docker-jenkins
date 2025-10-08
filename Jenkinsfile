pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // Jenkins credential ID
        DOCKER_IMAGE = "gourikulkarni/java-docker-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url:'https://github.com/Gouri5104/java-docker-jenkins.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh """
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                    docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Docker image successfully pushed to Docker Hub!"
        }
        failure {
            echo "Build failed. Check logs."
        }
    }
}
