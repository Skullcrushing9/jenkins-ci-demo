pipeline {
    agent any
    environment {
        APP_NAME = "demo-app"
        CONTAINER_NAME = "demo-container"
        PORT = "8081"
    }

    stages {

        stage('Checkout') {

            steps {

                checkout scm

            }

        }

        stage('Build Image') {

            steps {

                sh 'docker build -t ${APP_NAME}:${BUILD_NUMBER} .'

            }

        }

        stage('Deploy New Version') {

            steps {

                sh '''

                     docker rm -f ${CONTAINER_NAME} || true

                     docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${APP_NAME}:${BUILD_NUMBER}

                   '''

            }

        }

    }

}
