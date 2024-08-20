pipeline {
    agent any

    stages{
        stage('Build'){
            steps{
                sh "echo Building Stage1"
            }
        }

        stage('Test'){
            steps{
                sh "echo Testing Stage2"
            }
        }
        stage('Webhook test'){
            steps{
                sh "echo Testing webhook."
            }
        }
        stage('Deploy'){
            steps{
                sh "echo Deploying Stage3"
            }
        }
    }
}