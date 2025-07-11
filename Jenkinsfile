pipeline {
  agent any

  environment {
    IMAGE_NAME = "nbn-app"
    DOCKER_HUB_REPO = "mohdkhal/nbn-app"
  }

  stages {
    stage('Trivy Scan') {
      steps {
        sh 'trivy fs . || true'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker tag $IMAGE_NAME:latest $DOCKER_HUB_REPO:latest
            docker push $DOCKER_HUB_REPO:latest
          '''
        }
      }
    }
  }
}

