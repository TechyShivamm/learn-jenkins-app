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
                sh '''
                    ls -la
                    node --version
                    npm --version
                    sudo chown -R 666 "/.npm"
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
          steps {
            sh '''
              test -f build/index.html
              npm test
            '''
          }
        }
    }
    post {
      always {
        junit 'test-results/junit.xml'
      }
    }
}
