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
                ssh ubuntu@<JENKINS_SERVER_IP> "
                ansible-playbook \
                -i /home/ubuntu/ansible/inventory \
                /home/ubuntu/ansible/deploy.yml
                "
                '''
            }
        }
    }
}
