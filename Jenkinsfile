pipeline {

    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '672120083021'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-ecr'
                ]]) {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                    '''
                }
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t streamingapp-frontend ./frontend'
            }
        }

        stage('Build Auth Service') {
            steps {
                sh 'docker build -t streamingapp-auth ./backend/authService'
            }
        }

        stage('Build Streaming Service') {
            steps {
                sh 'docker build -t streamingapp-streaming ./backend/streamingService'
            }
        }

        stage('Build Admin Service') {
            steps {
                sh 'docker build -t streamingapp-admin ./backend/adminService'
            }
        }

        stage('Build Chat Service') {
            steps {
                sh 'docker build -t streamingapp-chat ./backend/chatService'
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag streamingapp-frontend:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-frontend:latest

                docker tag streamingapp-auth:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-auth:latest

                docker tag streamingapp-streaming:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-streaming:latest

                docker tag streamingapp-admin:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-admin:latest

                docker tag streamingapp-chat:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-chat:latest
                '''
            }
        }

        stage('Push Images to ECR') {
            steps {
                sh '''
                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-frontend:latest

                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-auth:latest

                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-streaming:latest

                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-admin:latest

                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/streamingapp-chat:latest
                '''
            }
        }
    }
}