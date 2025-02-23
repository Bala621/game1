pipeline {
    agent { label 'server-1' }
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/Bala621/game.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t mazeimage /home/Bala/workspace/game'
                sh 'sudo docker tag mazeimage bala246/mazeimage:latest'
                sh 'sudo docker tag mazeimage bala246/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Trivy Vulnerability Scan') {
            steps {
                script {
                    sh 'sudo trivy image vijaybala246/mazeimage:latest'
                    sh 'sudo trivy image bala246/mazeimage:${BUILD_NUMBER}'
                }
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push bala246/mazeimage:latest'
                sh 'sudo docker image push bala246/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /home/vijay/workspace/game/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
