pipeline {
    agent any

    environment {
        IMAGE = 'ghcr.io/Susmitha1469/week7-app:latest'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'

                sh '''
                    python3 -m venv .venv
                    .venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'

                sh '''
                    .venv/bin/pytest
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build -t $IMAGE .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging into GitHub Container Registry...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-ghcr',
                        usernameVariable: 'GH_USERNAME',
                        passwordVariable: 'GH_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "$GH_TOKEN" | docker login ghcr.io \
                        -u "$GH_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Docker image to GHCR...'

                sh '''
                    docker push $IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD pipeline failed.'
        }
    }
}