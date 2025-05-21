pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/nikilprasannaks/123.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t react-test .'
            }
        }

        stage('Run Tests in Docker') {
            steps {
                sh 'docker run --rm react-test npm test -- --watchAll=false'
            }
        }
    }
}
