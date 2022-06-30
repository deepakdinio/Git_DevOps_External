pipeline {
    agent any 
    environment {
        registryCredential = 'dockerhubcredential'
        imageName = 'deepakdinio/devops-bootcamp-external'
        dockerImage = ''
        }
    stages {
        stage('Run the tests') {
             agent {
                docker { 
                    image 'node:18-alpine3.15'
                    args '-e HOME=/tmp -e NPM_CONFIG_PREFIX=/tmp/.npm'
                    reuseNode true
                }
            }
            steps {
                echo 'Retrieve source from github. run npm install and npm test' 
            }
        }
        stage('Building image') {
            steps{
                script {
                    echo 'build the image'
                    docker.build $(environment.imageName):$(environment.BUILD_NUMBER)
                    echo "build complete"
                }
            }
            }
        stage('Push Image') {
            steps{
                script {
                    echo 'push the image to docker hub'
                    docker.push $(environment.imageName):$(environment.BUILD_NUMBER)
                    
                }
            }
        }     
         stage('deploy to k8s') {
             agent {
                docker { 
                    image 'google/cloud-sdk:latest'
                    args '-e HOME=/tmp'
                    reuseNode true
                        }
                    }
            steps {
             }
        }     
        stage('Remove local docker image') {
            steps{
                sh "docker rmi $imageName:latest"
                sh "docker rmi $imageName:$BUILD_NUMBER"
            }
        }
    }
}