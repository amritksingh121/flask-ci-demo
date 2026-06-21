pipeline {
    agent { label 'agent' }

    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub-username')
        DOCKERHUB_PASSWORD = credentials('dockerhub-password')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'python3 -m pytest tests/'
            }
        }

        stage('Lint') {
            steps {
                sh 'python3 -m flake8 app.py'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'python3 -m bandit -r app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USERNAME/flask-ci-demo:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                sh 'docker push $DOCKERHUB_USERNAME/flask-ci-demo:latest'
            }
        }
    }
}
