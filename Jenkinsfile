pipeline {
    agent any
    
    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning repository'
            }
        }
        stage('Build Docker Image ') {
            steps {
                echo 'Building Docker Image'

                sh 'docker build -t jenkins-demo-app .'
            }
        }
        stage('Test') {
            steps {
                echo 'No tests yet'
            }
        }
    } 
}
