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

        stage('Build Frontend') {
            steps {
                sh '''
                docker build -t streamingapp-frontend ./frontend
                '''
            }
        }

        stage('Build Auth Service') {
            steps {
                sh '''
                docker build -t streamingapp-auth ./backend/authService
                '''
            }
        }

        stage('Build Streaming Service') {
            steps {
                sh '''
                docker build -t streamingapp-streaming ./backend/streamingService
                '''
            }
        }

        stage('Build Admin Service') {
            steps {
                sh '''
                docker build -t streamingapp-admin ./backend/adminService
                '''
            }
        }

        stage('Build Chat Service') {
            steps {
                sh '''
                docker build -t streamingapp-chat ./backend/chatService
                '''
            }
        }

    }
}