pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sagar592/sample-website:latest"
        GIT_URL = "https://github.com/Harihshshyam/sample-website.git"
        DOCKER_CREDENTIALS = credentials('dockerhub-cred')
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: "${GIT_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Add your Kubernetes deployment steps here
                // For example:
                // sh "kubectl apply -f deployment.yaml"
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
