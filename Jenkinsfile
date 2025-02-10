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
                    npm ci
                    npm run build
                    ls -la
                '''
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
                script {
                    // Run tests inside Docker and save junit.xml outside the container
                    sh '''
                        npm install --save-dev mocha mocha-junit-reporter
                        npm run test -- --reporter mocha-junit-reporter --reporter-options mochaFile=test-results/junit.xml
                    '''
                }
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
