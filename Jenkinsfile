pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'ilhamnurizha@gmail.com'
        DOCKER_IMAGE = 'ilhamnurizha/nginx-test:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/Gegee09/TEST-CICD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                sshagent(['target-server-creds']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no root@192.168.1.135 << EOF
                        docker pull $DOCKER_IMAGE
                        docker rm -f nginx-container || true
                        docker run -d --name nginx-container -p 83:80 $DOCKER_IMAGE
                        EOF
                    '''
                }
            }
        }
    }
}
