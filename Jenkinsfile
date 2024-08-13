pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'mywebapp'
        EC2_IP = '34.212.101.247'
        SSH_KEY = '/path/to/your/private/key'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/jaydeep123s/mycompany.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_NAME}:latest'
                    sh 'docker push ${DOCKER_IMAGE_NAME}:latest'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh '''
                    ssh -i ${SSH_KEY} ec2-user@${EC2_IP} << EOF
                    docker pull ${DOCKER_IMAGE_NAME}:latest
                    docker stop mywebapp || true
                    docker rm mywebapp || true
                    docker run -d -p 80:80 --name mywebapp ${DOCKER_IMAGE_NAME}:latest
                    EOF
                    '''
                }
            }
        }
    }
}
