pipeline {
    agent any

    stages{
        stage('Build Frontend'){
            steps{
                sh "echo Building frontend"
                sh "cd frontend && npm install && npm run build"

            }
        }
        stage('Deploy Frontend'){
            steps{
                sh "echo Deploying Frontend"
                script{
                    withAWS(region: 'us-east-1', credentials: 'AWS_CREDENTIALS'){
                        sh "aws sync frontend/dist <URL HERE>"
                    }
                }
            }
        }
    }
}