pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    // Checking the system's initial state
                    sh 'ls -la'
                    sh 'node --version'
                    sh 'npm --version'
                }
                
                // Ensure package-lock.json exists before running npm ci
                script {
                    if (!fileExists('package-lock.json')) {
                        error 'package-lock.json is missing. npm ci requires it.'
                    }
                }
                
                sh '''
                    echo "Running npm ci..."
                    npm ci --no-cache
                    echo "Build process completed."
                    ls -la
                '''
            }
        }
    }
}