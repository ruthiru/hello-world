pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("ruthiru/hello-world")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'github') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy to Minikube') {
            steps {
                sh """
                kubectl set image deployment/hello-world-deployment hello-world=ruthiru/hello-world:latest
                kubectl rollout status deployment/hello-world-deployment
                """
            }
        }
    }
}
