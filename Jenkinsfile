pipeline {
    agent any

    environment {
    DOCKER_IMAGE = 'ghoi73787-ship-it/sample-app'  // Updated with your actual username
    DOCKER_HUB_CRED = 'docker-hub-credentials'
}

    stages {
        stage('Clone Repository') {
            steps {
                // Jenkins automatically pulls the code if linked to the GitHub repo
                echo 'Source code pulled successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                script {
                    // Uses secured credentials saved in Jenkins
                    withDockerRegistry(credentialsId: DOCKER_HUB_CRED, url: '') {
                        sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }

    post {
        always {
            // Cleanup local images after pushing to keep disk space clean
            sh "docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest || true"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
