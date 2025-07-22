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
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh 'npm test'
            }
        }

        stage('E2E Tests') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.54.0-noble'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules\.bin\serve -s build
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
