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
                    def netlifySiteID = '874caac1-235f-4e36-a524-af99313e038f'
                    def netlifyAccessToken = 'nfp_ptqanm6NWhv6rz8oTWurNAncvsVhrJZdbc9c'
                    sh '''
                    npm install netlify-cli --save-dev
                    npx netlify deploy --site ${netlifySiteID} --auth ${netlifyAccessToken} --dir ./build --prod
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
