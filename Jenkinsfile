pipeline {
    agent any
    stages {
                agent {
                    docker {
                        image 'node:18-alpine'
                        reuseNode true
                    }
        stage('Build') {

            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm -version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){

            steps {
                echo 'Test stage'
                sh '''
                    test -f build/index.html
                    ls -la
                    npm -version
                '''
            }
        }
    }
}
