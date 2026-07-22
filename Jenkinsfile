pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mahesh26666/sample-app'
        DOCKER_HUB_CRED = 'docker-hub-credentials'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Build with both BUILD_NUMBER and latest tags
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CRED}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        always {
            // Clean up both local Docker images off the agent
            sh "docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest || true"
        }
    }
}
