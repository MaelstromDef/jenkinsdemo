pipeline {
    agent any

    stages{
        stage('Build Frontend'){
            steps{
                sh "echo Building Frontend"
                sh "cd frontend && npm install && npm run build"

            }
        }
        stage('Deploy Frontend'){
            steps{
                sh "echo Deploying Frontend"
                script{
                    withAWS(region: 'us-east-1', credentials: 'AWS_CREDENTIALS'){
                        sh "aws s3 sync frontend/dist s3://bjgomes-bucket-sdet"
                    }
                }
            }
        }
        stage('Build Backend'){
            steps{
                sh "echo Building Backend"
                sh "cd demo && mvn clean install"
            }
        }
    }
}