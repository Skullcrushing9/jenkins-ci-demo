pipeline {
    agent any
    environment {
        APP_NAME = "demo-app"
        PORT = "8085"
        IMAGE_NAME = "gamemaster007/demo-app"
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

                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'

            }

        }
        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds' ,usernameVariable: 'DH_USER' ,passwordVariable: 'DH_PASS')]) {
                sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
                }
            }
        }   
        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
        
        stage('Deploy New Version') {

            steps {

                sh '''

                     docker rm -f $APP_NAME || true

                     docker run -d --name $APP_NAME -p $PORT:80 $IMAGE_NAME:$BUILD_NUMBER

                   '''

            }

        }

    }

}
