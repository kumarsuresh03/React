pipeline {
    agent any
    environment {
        dockerRegistry = "https://index.docker.io/v1/"
        dockerCreds = credentials('dockerhub-credentials')  // Your Docker Hub credentials
        nodeImage = 'mern-node'
    }
    stages {
        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry(dockerRegistry, 'dockerhub-credentials') {
                        echo "Logged in to Docker Hub"
                    }
                }
            }
        }
       
        stage('Build Node.js') {
            steps {
                script {
                    def nodePath = "simple-reactjs-app-master"
                    def dockerfileNode = "Dockerfile"
                    if (fileExists(simple-reactjs-app-master)) {
                        bat "docker build -f ${dockerfileNode} -t ${nodeImage}:latest ${nodePath}"
                        bat "docker tag ${nodeImage}:latest sureshnangina/mern-node:latest"
                    } else {
                        error "Node.js directory ${nodePath} not found"
                    }
                }
            }
        }
        
        stage('Push Node.js') {
            steps {
                script {
                    docker.withRegistry(dockerRegistry, 'dockerhub-credentials') {
                        echo "Pushing Node.js image to Docker Hub"
                        bat "docker push sureshnangina/mern-node:latest"
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
