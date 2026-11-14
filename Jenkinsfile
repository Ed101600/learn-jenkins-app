pipeline {
    agent {
        docker {
            image 'node:22.23.5-alpine'
        }
    }

    environment {
        CI = 'true'
    }

    stages {
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
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
