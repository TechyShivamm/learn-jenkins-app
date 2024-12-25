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
                    // Fix permission issue for npm cache
                    echo "Setting npm cache directory..."
                    sh 'mkdir -p /home/jenkins/.npm'
                    sh 'npm config set cache /home/jenkins/.npm --global'
                    sh 'npm cache clean --force'
                    
                    // Checking the system's initial state
                    echo 'Initial system state:'
                    sh 'ls -la'
                    sh 'node --version'
                    sh 'npm --version'

                    // Ensure package-lock.json exists before running npm ci
                    if (!fileExists('package-lock.json')) {
                        error 'package-lock.json is missing. npm ci requires it.'
                    }
                }

                script {
                    echo "Running npm ci..."
                    try {
                        sh '''
                            rm -rf node_modules
                            npm ci --no-cache
                        '''
                    } catch (Exception e) {
                        error "npm ci failed: ${e.getMessage()}"
                    }
                }

                script {
                    echo "Build process completed."
                    sh 'ls -la'  // List files after build process to verify
                }
            }
        }
    }
}