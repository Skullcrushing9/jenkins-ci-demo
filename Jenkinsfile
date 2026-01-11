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

                docker run -d \

                -p 8081:80 \

                --name demo-app-${BUILD_NUMBER} \

                demo-app:${BUILD_NUMBER}

                '''

            }

        }

    }

}
