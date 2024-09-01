pipeline {
    agent any

    environment {
        // Define the IMAGE_TAG environment variable using Groovy string interpolation
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git credentialsId: 'github', 
                    url: 'https://github.com/Saitejamahi/endtoend-cicd.git', 
                    branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    // Build the Docker image
                    sh '''
                    echo "Building Docker Image"
                    docker build -t saitejamahi/endtoend-cicd:${IMAGE_TAG} .
                    '''
                }
            }
        }

        stage('Push the Artifacts') {
            steps {
                script {
                    // Push the Docker image to the repository
                    sh '''
                    echo "Pushing Docker Image to Repo"
                    docker push saitejamahi/endtoend-cicd:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Checkout k8s Manifest SCM') {
            steps {
                // Checkout Kubernetes manifests from GitHub
                git credentialsId: 'github', 
                    url: 'https://github.com/Saitejamahi/endtoend-cicd.git', 
                    branch: 'main'
            }
        }

        stage('Update k8s Manifest & Push to Repo') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        // Update Kubernetes manifest and push changes
                        sh '''
                        echo "Updating Kubernetes Manifest"
                        cat deploy.yaml
                        sed -i "s/32/${IMAGE_TAG}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m "Updated the deploy yaml with new image tag | Jenkins Pipeline"
                        git remote -v
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Saitejamahi/endtoend-cicd.git HEAD:main
                        '''
                    }
                }
            }
        }
    }
}
