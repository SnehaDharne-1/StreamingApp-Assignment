pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '672120083021'
        ECR_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }

    stages {

        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t streamingapp-frontend ./frontend'
            }
        }

        stage('Build Auth Image') {
            steps {
                sh 'docker build -t streamingapp-auth ./backend/authService'
            }
        }

        stage('Build Streaming Image') {
            steps {
                sh 'docker build -t streamingapp-streaming -f ./backend/streamingService/Dockerfile ./backend'
            }
        }

        stage('Build Admin Image') {
            steps {
                sh 'docker build -t streamingapp-admin -f ./backend/adminService/Dockerfile ./backend'
            }
        }

        stage('Build Chat Image') {
            steps {
                sh 'docker build -t streamingapp-chat -f ./backend/chatService/Dockerfile ./backend'
            }
        }

        stage('Build Complete') {
            steps {
                echo 'All Docker Images Built Successfully'
            }
        }

    }
}