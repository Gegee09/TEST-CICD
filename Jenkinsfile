kpipeline {
    agent any

    environment {
        IMAGE = "dockerhub_user/nginx-ci:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kamu/repo.git'
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

