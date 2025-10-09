pipeline {
  agent any
  options { timestamps() }

  environment {
    DOCKER_IMAGE = 'your-dockerhub-username/java-docker-app'
  }

  stages {
    stage('Build JAR') {
      steps {
        bat 'mvn -v'
        bat 'mvn clean package'
      }
    }

    stage('Docker preflight') {
      steps {
        bat 'docker version'
      }
    }

    stage('Build Docker Image') {
      steps {
        bat 'docker build -t %DOCKER_IMAGE%:latest -t %DOCKER_IMAGE%:%BUILD_NUMBER% .'
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          bat '''
            echo %DH_PASS% | docker login -u %DH_USER% --password-stdin
            docker push %DOCKER_IMAGE%:latest
            docker push %DOCKER_IMAGE%:%BUILD_NUMBER%
          '''
        }
      }
    }
  }

  post {
    success { echo "Pushed: ${env.DOCKER_IMAGE}:latest and :${env.BUILD_NUMBER}" }
    failure { echo 'Build failed. Check logs.' }
  }
}

