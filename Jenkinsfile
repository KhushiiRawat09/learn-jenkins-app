pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'c357ba05-fc34-4094-b2c6-243033e5cef7'
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
                    echo "Build Stage"
                    echo "Helloo"
                    npm ci
                    npm run build
                    ls -la build
                '''
            }
        }


        stage('Tests') {
            parallel {

                stage('Unit Test') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "Unit Test Stage"
                            test -f build/index.html
                            npm test
                        '''
                    }
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
                sh '''
                    npm install netlify-cli
                    netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/**/*.xml'
        }
    }
}
