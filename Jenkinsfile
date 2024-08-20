pipeline {
    agent any

    stages{
        stage('Build Frontend'){
            steps{
                sh "echo Building frontend"
                sh "cd frontend && npm install && npm run build"
            }
        }
    }
}