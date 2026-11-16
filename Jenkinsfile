pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:22.23.5-alpine'
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

        stage(Run Tests') {
            Parallel {
                stage('Run Jest Tests') {
                    agent {
                        docker {
                            image 'node:22.23.5-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            npm run test:jest
                        '''
                    }
                }

                stage('Run Playwright Tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.42.0-focal'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            npm run test:playwright
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}
