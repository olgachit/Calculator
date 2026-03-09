
pipeline {
    agent any

    tools {
        jdk 'JDK 21'
        maven 'Maven 3.9.12'
    }

    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
        DOCKERHUB_CREDENTIALS_ID = 'docker_hub'
        DOCKERHUB_REPO = 'olgachi/calculator'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                        url: 'https://github.com/olgachit/Calculator.git'
            }
        }

        stage('Debug Docker') {
            steps {
                sh '''
        echo "PATH=$PATH"
        which docker || true
        ls -l /opt/homebrew/bin/docker || true
        docker --version || true
        '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }

    }
}
