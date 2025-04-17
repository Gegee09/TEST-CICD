pipeline {
    agent any

    environment {
        IMAGE = "ilhamnurizha/nginx-test:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/Gegee09/TEST-CICD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                        docker push $IMAGE
                    """
                }
            }
        }
    }
}

