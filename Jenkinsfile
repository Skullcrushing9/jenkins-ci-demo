pipeline {

    agent any

    stages {

        stage('Checkout') {

            steps {

                checkout scm

            }

        }

        stage('Build Image') {

            steps {

                sh 'docker build -t demo-app:${BUILD_NUMBER} .'

            }

        }

        stage('Deploy New Version') {

            steps {

                sh '''

                     docker rm -f demo-container || true

                     docker run -d --name demo-container -p 8081:80 demo-app:8

                   '''

            }

        }

    }

}
