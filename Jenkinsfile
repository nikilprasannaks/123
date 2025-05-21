pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Explicitly specify main branch and clean workspace
                git branch: 'main', 
                     url: 'https://github.com/nikilprasannaks/123.git',
                     credentialsId: 'your-github-credentials',  // If private repo
                     poll: false  // Disable SCM polling if not needed
                sh 'ls -la'  // Debug: verify files were checked out
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Verify Docker is installed and accessible
                    sh 'docker --version'
                    
                    // Build with cache and proper tagging
                    sh 'docker build -t react-app:${BUILD_NUMBER} -t react-app:latest .'
                    
                    // Clean up intermediate containers
                    sh 'docker system prune -f'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests with proper resource limits
                    sh '''
                    docker run --rm \
                        -e CI=true \
                        react-app:${BUILD_NUMBER} \
                        npm test -- --watchAll=false --coverage
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images after build
            sh 'docker rmi react-app:${BUILD_NUMBER} || true'
            sh 'docker rmi react-app:latest || true'
            
            // Archive test results if any
            junit '**/test-results.xml'  // Update path to your test results
            archiveArtifacts '**/coverage/**'  // Archive coverage reports
        }
        
        failure {
            // Notifications for failed builds
            mail to: 'nikilprasannaks.22cse@kongu.edu',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Check ${env.BUILD_URL}"
        }
    }
}
