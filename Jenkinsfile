pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'  // Use the ID of your Docker Hub credentials
        DOCKER_IMAGE = 'olakitanankinya/my-node-app'
        DOCKER_REGISTRY_URL = 'https://index.docker.io/v1/'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/olakitanakinya/ci-cd.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry(DOCKER_REGISTRY_URL, DOCKER_CREDENTIALS_ID) {
                        sh 'docker tag ${DOCKER_IMAGE} ${DOCKER_IMAGE}:latest'
                        sh 'docker push ${DOCKER_IMAGE}:latest'
                    }
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}

