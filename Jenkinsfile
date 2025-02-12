pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd9ab072d-8b55-440c-921d-dfbb6e7d6786'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                    npm run build
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
                    sh '''
                        npm install --save-dev jest jest-junit
                        npm run test
                    '''
                }
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploy to production. Site ID = $NETLIFY_SITE_ID"
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
