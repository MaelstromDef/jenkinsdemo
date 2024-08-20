pipeline {
    agent any

    stages{
        stage('Build Frontend'){
            steps{
                sh "echo Building Frontend"
                sh "cd frontend/vite-project && npm install && npm run build"

            }
        }
        stage('Deploy Frontend'){
            steps{
                sh "echo Deploying Frontend"
                script{
                    try{
                        withAWS(region: 'us-east-1', credentials: 'AWS_CREDENTIALS'){
                            sh "aws s3 sync frontend/vite-project/dist s3://bjgomes-bucket-sdet"
                        }
                    }catch(Exception e){
                        echo "${e}"
                        throw e
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
        stage('Test Backend'){
            steps{
                sh "echo Testing Backend"
                sh "cd demo && mvn test"
            }
        }
        stage('Deploy Backend'){
            steps{
                sh "echo Deploying Backend"
                script{
                    try{
                        withAWS(region: 'us-east-1', credentials: 'AWS_CREDENTIALS'){
                            sh "aws s3 sync demo/target/*.jar s3://bjgomes-bucket-sdet-backend"
                            sh "aws elasticbeanstalk create-application-version --application-name ahuggins-jenkins-demo --version-label 0.0.1 --source-bundle S3Bucket="bjgomes-bucket-sdet-backend",S3Key="*.jar""
                            sh "aws elasticbeanstalk update-environment --environment-name your-environment-name --version-label your-version-label"
                        }
                    }catch(Exception e){
                        echo "${e}"
                        throw e
                    }
                }
            }
        }
    }
}