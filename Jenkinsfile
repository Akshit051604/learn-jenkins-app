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
                        # Install testing dependencies
                        npm install --save-dev mocha mocha-junit-reporter

                        # Run tests and generate JUnit XML file in test-results directory
                        mkdir -p test-results  # Make sure the directory exists
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
