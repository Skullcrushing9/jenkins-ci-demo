pipeline {
  agent any

  environment {
    APP_NAME = "demo-app"
    IMAGE_NAME = "gamemaster007/demo-app"
    PORT = "8085"
    TAG = "${BUILD_NUMBER}"
  }

  stages {

    stage('Checkout') {
      steps {
        cleanWs()
        checkout scm
      }
    }

    stage('Build Image') {
      steps {
        sh '''
          echo "Building image..."
          docker build -t $IMAGE_NAME:$TAG .
        '''
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          sh '''
            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
            docker push $IMAGE_NAME:$TAG
          '''
        }
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          echo "Deploying new version..."
          docker rm -f $APP_NAME || true
          docker run -d --name $APP_NAME -p $PORT:80 $IMAGE_NAME:$TAG
        '''
      }
    }

    stage('Health Check') {
      steps {
        sh '''
          echo "Checking app..."
          sleep 3
          curl -I http://localhost:$PORT || exit 1
        '''
      }
    }

  }

  post {
    success {
      echo "Deployment successful: $IMAGE_NAME:$TAG"
    }
    failure {
      echo "Pipeline failed. Check Console Output."
    }
    always {
      sh '''
        echo "Cleaning unused dangling images..."
        docker image prune -f || true
      '''
    }
  }
}
