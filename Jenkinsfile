pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'andreyrozanov/todo:latest'
        USER = 'ubuntu'
        DOCKERHUB_CREDENTIALS = 'andreyrozanov-dockerhub'
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        SSH_KEY = 'aws-ssh'
        PUBLIC_DNS = 'ec2-34-205-146-231.compute-1.amazonaws.com'
        DEPLOY_PATH = '/home/ubuntu/todo-app'
  }
    
    
    stages{
        
    //    stage('Build and Push Docker Image') {
    //         steps {
    //             //sh 'docker version'
    //             //sh 'docker run hello-world'
    //             sh 'docker build -t $DOCKER_IMAGE .'
    //             withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
    //                 sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
    //             }
    //             sh 'docker push $DOCKER_IMAGE'
    //         }
    //     }

        stage('Deploy to EC2') {
            steps {
                sshagent([SSH_KEY]) {
                sh """
                scp -o StrictHostKeyChecking=no -r docker-compose.yml ${USER}@${PUBLIC_DNS}:${DEPLOY_PATH}docker-compose.yml
                ssh -o StrictHostKeyChecking=no ${USER}@${PUBLIC_DNS} "cd ${DEPLOY_PATH} && \
                docker-compose down && \
                docker pull ${DOCKER_IMAGE} && \
                docker-compose up -d"
                """
        }
      }
    }
      
    }
}
