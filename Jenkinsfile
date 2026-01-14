pipeline {
    agent any
    environment {
        APP_NAME = "demo-app"
        CONTAINER_NAME = "demo-container"
        PORT = "8081"
        DOCKERHUB_USER = "gamemaster007"
        IMAGE = "${DOCKERHUB_USER}/${APP_NAME}"
    }

    stages {

        stage('Checkout') {

            steps {

                checkout scm

            }

        }

        stage('Build Image') {

            steps {

                sh 'docker build -t ${IMAGE}:${BUILD_NUMBER} .'

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
                sh 'docker push ${IMAGE}:${BUILD_NUMBER}'
            }
        }
        
        stage('Deploy New Version') {

            steps {

                sh '''

                     docker rm -f ${CONTAINER_NAME} || true

                     docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${IMAGE}:${BUILD_NUMBER}

                   '''

            }

        }

    }

}
