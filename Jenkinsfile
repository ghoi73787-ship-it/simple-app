pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mahesh26666/sample-app'
        DOCKER_HUB_CRED = 'docker-hub-credentials'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Clone Repository') {
            steps {
                echo 'Repository Cloned'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
            }
        }
        stage('Docker Login & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: "${DOCKER_HUB_CRED}", url: '') {
                        sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }
    post {
        always {
            sh "docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER} || true"
        }
    }
}
