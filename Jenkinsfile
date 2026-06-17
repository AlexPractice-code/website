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
                docker rm -f test-container || true

                docker run -d \
                --name test-container \
                -p 8081:80 \
                $IMAGE_NAME

                sleep 10

                curl -f http://localhost:8081

                docker rm -f test-container
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

            when {
                branch 'master'
            }

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
