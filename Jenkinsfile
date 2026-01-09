pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checing out'

                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building application'
            }
        }
        stage('Build Docker Image ') {
            steps {
                echo 'Building Docker Image'

                sh 'docker build -t jenkins-demo-app:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests'
            }
        }
    } 
}
