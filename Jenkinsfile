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
    
        stage('docker build') {
            agent {   
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args '-u root --entrypoint=""'
                }
            }
            steps {
                sh 'docker build -t learn-jenkins-app .'
            }
        }

        stage('Deploy to AWS'){
            agent {
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args '-u root --entrypoint=""'
                }
            }
            environment {
                AWS_S3_BUCKET_NAME = 'lern-jenkins-app20260726'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-key', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        echo "Deploying to AWS S3"
                        aws s3 sync  build s3://$AWS_S3_BUCKET_NAME
                    '''
                }
            }
        }
    }
}