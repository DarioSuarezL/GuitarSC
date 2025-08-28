pipeline {
    agent any

    tools {
        nodejs 'N22.17'
    }

    stages{
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }


        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo Deploying application...'
            }
        }
    }
}