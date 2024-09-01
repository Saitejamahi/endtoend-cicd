pipeline {
  agent any {
    environment
    {
      IMAGE-TAG = "$(BUILD_NUMBER)"
    }
    stages {
      stage('Checkout')
      {
        steps{
            git credintialsId : '' ,
            url: 'https://github.com/Saitejamahi/endtoend-cicd',
            branch: 'main'
        }
      }
      stage('Build Docker')
      {
        steps {
          script {
            sh '''
            echo 'Build Docker Image'
            docker build -t 
