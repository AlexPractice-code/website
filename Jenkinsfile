pipeline {

    agent { label 'build-agent' }

    environment {
        IMAGE_NAME = "anmolnegi/webapp:latest"
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                docker images | grep webapp
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                ansible-playbook \
                -i /home/ubuntu/inventory \
                /home/ubuntu/deploy.yml
                '''
            }
        }
    }
}
