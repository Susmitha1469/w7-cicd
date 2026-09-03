pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                echo 'Running tests...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
            }
        }
    }
}