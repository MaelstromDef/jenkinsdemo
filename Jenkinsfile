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
                            sh "aws s3 sync frontend/vite-project/dist s3://ahuggins-jenkins-test"
                        }
                    }catch(Exception e){
                        echo "${e}"
                        throw e
                    }
                }
            }
        }
        stage('Test Backend'){
            steps{
                sh "echo Testing Backend"
                sh "cd demo && mvn test"
            }
        }
        stage('Build Backend'){
            steps{
                sh "echo Building Backend"
                sh "cd demo && mvn clean install"
            }
        }
        stage('Deploy Backend'){
            steps{
                sh "echo Deploying Backend"
                script{
                    try{
                        withAWS(region: 'us-east-1', credentials: 'AWS_CREDENTIALS'){
                            sh "aws s3 sync demo/target/*.jar s3://ahuggins-jenkins-test-backend"
                            sh "aws elasticbeanstalk create-application-version --application-name ahuggins-jenkins-demo --version-label 0.0.1 --source-bundle S3Bucket="ahuggins-jenkins-test-backend",S3Key="*.jar""
                            sh "aws elasticbeanstalk update-environment --environment-name ahuggins-jenkins-demo-env --version-label 0.0.1"
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