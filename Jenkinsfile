pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'andreyrozanov/todo:latest'
        USER = 'ubuntu'
        DOCKERHUB_CREDENTIALS = 'andreyrozanov-dockerhub'
  }
    
    
    stages{
        
       stage('Build and Push Docker Image') {
            steps {
                sh 'docker version'
                sh 'docker build -t $DOCKER_IMAGE .'
                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                }
                sh 'docker push $DOCKER_IMAGE'
            }
        }

      
    }
}
