pipeline {
  agent any

  environment {
    IMAGE_NAME = "nbn-app"
    DOCKER_HUB_REPO = "mohdkhaleelk/nbn-app"  // Replace with your DockerHub repo name
  }

  stages {
    stage('Clone Repo') {
      steps {
        git credentialsId: 'github-creds', url: 'https://github.com/mohdkhal/nbn-ci-pipeline.git'
      }
    }

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

