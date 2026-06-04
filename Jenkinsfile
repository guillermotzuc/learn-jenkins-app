pipeline {
    agent any

    stages {
        // This is a comment
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
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        /*
         Multiline comment
        */
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                 test -f build/index.html
                 npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.60.0-noble'
                    reuseNode true
                    args '-u root'  // Run as root to avoid permission issues
                }
            }
            steps {
                sh '''
                 npm install serve
                 node_modules/.bin/serve -s build &
                 sleep 10
                 export PLAYWRIGHT_BROWSERS_PATH=/usr/lib/playwright
                 npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}
