pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Harihshshyam/sample-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-website:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop sample-container || true
                docker rm sample-container || true
                docker run -d --name sample-container -p 80:80 sample-website:latest
                '''
            }
        }
    }
}
