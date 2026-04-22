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
                    echo "Build Stage"
                    echo "Hello"
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
    }

    post {
        always {
            junit 'test-results/**/*.xml'
        }
    }
}
