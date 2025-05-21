pipeline {
    agent {
        docker {
            image 'node:18' // Use your preferred Node version
            args '-v $HOME/.npm:/root/.npm' // Optional: cache npm
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/nikilprasannaks/123.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Unit Tests') {
            steps {
                sh 'npm test -- --watchAll=false'
            }
        }
    }
    post {
        always {
            junit '**/test-results/**/*.xml' // Optional: if using test reporters
        }
    }
}
