pipeline {
    agent any

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                '''
            }
        }
    }
    stage('docker build') {
        agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
        steps {
            sh 'docker build -t learn-jenkins-app .'
        }
    }

}