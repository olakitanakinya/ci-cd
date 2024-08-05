pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub-credentials'
        DOCKER_IMAGE = 'olakitanankinya/my-node-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/olakitanakinya/ci-cd.git',
                    credentialsId: "${env.GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${env.DOCKERHUB_CREDENTIALS_ID}") {
                        docker.image("${env.DOCKER_IMAGE}").push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'k8s/deployment.yaml',
                        kubeconfigId: 'kubernetes-config'
                    )
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

