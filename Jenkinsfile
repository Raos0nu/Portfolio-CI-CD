pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your_dockerhub_username/portfolio:latest'
        REGISTRY_CREDENTIAL = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git('https://github.com/Raos0nu/Portfolio-CI-CD.git')
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', REGISTRY_CREDENTIAL) {
                        dockerImage.push('latest') 
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
