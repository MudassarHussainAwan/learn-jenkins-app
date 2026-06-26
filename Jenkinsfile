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
        variables {
            AWS_S3_BUCKET_NAME = 'lern-jenkins-app20260726'
            AWS_ACCESS_KEY_ID = credentials('aws-key').username
            AWS_SECRET_ACCESS_KEY = credentials('aws-key').password
        }
        steps {
            withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                sh '''
                    aws --version
                    echo "Deploying to AWS S3"
                    aws s3 sync  build s3://$AWS_S3_BUCKET_NAME
                '''
            }

        }
    }


}