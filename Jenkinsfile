pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/scrambi/web1.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t scramby/scram .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh """
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $DOCKER_USER/scram:latest
                    """
                }
            }
        }
    }
}
