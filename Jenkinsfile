pipeline
{
  agent any
    environment
    {
      IMAGE_TAG = "${env.BUILD_NUMBER}"
    }
    stages 
      {
      stage('Checkout')
        {
        steps
          {
            git
            ( credentialsId:'github',
            url: 'https://github.com/Saitejamahi/endtoend-cicd.git',
            branch:'main'
             )
          }
        }
      stage('Build Docker')
      {
        steps
        {
          script 
          {
            sh '''
            echo 'Build Docker Image'
            docker build -t Saitejamahi/endtoend-cicd:${IMAGE_TAG} .
          
            '''
            }
          }
        }
        stage('Push the artifacts')
        {
          steps
          {
            script
            {
               sh '''
              echo 'push to Repo'
              docker push Saitejamahi/endtoend-cicd : "${IMAGE_TAG}"
              '''
           }
        }
      }
      
      stage('checkout k8s manifest SCM')
      {
        steps
        {
          git( credintialId: 'github',
            branch: 'main'
              )
        }
      }
      stage('Update k8s manifest & push to Rep')
      {
        steps
        {
          script
          {
            withCredentials([usernamePassword(credintalsId: 'github',passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')])
            {
              sh '''
              echo 'Updating K8s manifest'
              cat deploy.yaml
              sed -i "s/32/${BUILD_NUMBER}/g" deploy.yaml
              cat deploy.yaml
              git add deploy.yaml
              git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
              git remote -v
              git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Saitejamahi/endtoend-cicd.git HEAD: main
              '''
              }
            }
          }
        }
      }
    }
  
