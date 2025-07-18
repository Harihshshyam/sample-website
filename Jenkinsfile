pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sagar592/sample-website:${env.BUILD_ID}"
        GIT_URL = "https://github.com/Harihshshyam/sample-website.git"
    }

    stages {
        stage('Clone Code') {
            steps {
                git url: "${GIT_URL}",
                    branch: 'main'
            }
        }

        stage('Build & Push') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                withKubeConfig([credentialsId: 'my-kubeconfig']) {
                    sh "kubectl set image deployment/sample-website-deployment sample-website=${DOCKER_IMAGE}"
                    sh "kubectl rollout status deployment/sample-website-deployment"
                }
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
