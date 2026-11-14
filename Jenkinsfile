pipeline {
    /*
    agent {
        docker {
            image 'node:22.23.5-alpine'
        }
    }

    environment {
        CI = 'true'
    }
    */

    stages {
        /*
        stage('Build') {
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
        stage('Test') {
            steps {
                sh '''
                     find ./build/ -type f -name "index.html"
                     npm test
                '''
            }
        }
        
        */
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.64.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                     npm install -g serve
                     serve -s build
                     npx playwright test 
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
