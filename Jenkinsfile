pipeline {
    agent any

    environment {
        IMAGE_NAME = "lucifer1503/hello-app"
        IMAGE_TAG = "latest"
        DOCKER_HUB = credentials('dockerhub-creds')
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Pratik1503/hello-app.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
                sh 'docker build -t lucifer1503/hello-app:latest .'
                sh 'docker push lucifer1503/hello-app:latest'
            }
        }

        stage('Push Docker Image') {
            steps {	
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Deploy with Helm') {
            steps {
                sh 'helm upgrade --install hello-release hello-chart'
            }
        }
    }
}
