pipeline {

    agent none

    environment {
        IMAGE_NAME = "anmolnegi/webapp:latest"
    }

    stages {

        stage('Build') {
            agent { label 'build-agent' }
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Test') {
            agent { label 'build-agent' }
            steps {
                sh '''
                docker images | grep webapp
                '''
            }
        }

        stage('Push Image') {
            agent { label 'build-agent' }
            steps {
                sh '''
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy') {
            agent any
            steps {
                sh '''
                ansible-playbook \
                -i /home/ubuntu/ansible/inventory \
                /home/ubuntu/ansible/deploy.yml
                '''
            }
        }
    }
}
