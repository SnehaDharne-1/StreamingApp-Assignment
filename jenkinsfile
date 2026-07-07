pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Project Information') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

    }
}