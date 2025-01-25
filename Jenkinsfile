pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "scramby/scram"  // Имя образа без тега
        VERSION = "v${BUILD_NUMBER}"    // Используем номер сборки как версию
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/scrambi/web1.git'
            }
        }

        stage('Build Image with Buildah') {
            steps {
                script {
                    sh """
                    buildah bud -t ${DOCKER_IMAGE}:${VERSION} .
                    """
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh """
                        buildah login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD docker.io
                        buildah push ${DOCKER_IMAGE}:${VERSION} docker.io/${DOCKER_IMAGE}:${VERSION}
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Обновляем версию образа в deployment.yaml
                    sh """
                    sed -i 's|scramby/scram:.*|${DOCKER_IMAGE}:${VERSION}|g' deployment.yaml
                    kubectl apply -f deployment.yaml
                    """
                }
            }
        }
    }
}
