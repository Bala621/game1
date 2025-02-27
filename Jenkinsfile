pipeline {
    agent { label 'server-1' }
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/Bala621/game.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                // Build the Docker image using the correct tag format
                sh 'sudo docker build -t mazeimage ${WORKSPACE}' 
                // Tag the image with the correct repository and version
                sh 'sudo docker tag mazeimage bala246/game:latest'  // Tag with latest
                sh 'sudo docker tag mazeimage bala246/game:${BUILD_NUMBER}'  // Tag with build number
            }
        }
        stage('Trivy Vulnerability Scan') {
            steps {
                script {
                    // Run Trivy scans on both tags
                    sh 'sudo trivy image bala246/game:latest'
                    sh 'sudo trivy image bala246/game:${BUILD_NUMBER}'
                }
            }
        }
        stage('Push the Docker image') {
            steps {
                // Push the Docker images to DockerHub
                sh 'sudo docker image push bala246/game:latest'
                sh 'sudo docker image push bala246/game:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /root/jenkins/workspace/game/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}