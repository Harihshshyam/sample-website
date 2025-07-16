pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/Harihshshyam/sample-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-website .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f techvision-container || true'
                sh 'docker run -d --name sample-website -p 8080:80 sample-website'
            }
        }
    }
}

