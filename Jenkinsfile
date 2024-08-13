pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'mywebapp'
        EC2_IP = 'YOUR_EC2_PUBLIC_IP'
        SSH_KEY = '/path/to/your/private/key'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/jaydeep123s/mywebapp.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
                }
            }
        }

       
    environment {
        DOCKER_IMAGE_NAME = 'your-docker-repo/your-image-name'
        DOCKER_REGISTRY_CREDENTIALS_ID = 'your-docker-credentials-id'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker registry
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDENTIALS_ID}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin'
                    }
                    
                    // Tag the Docker image with the 'latest' tag
                    sh 'docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_NAME}:latest'
                    
                    // Push the Docker image to the registry
                    sh 'docker push ${DOCKER_IMAGE_NAME}:latest'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Example deployment stage
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_IP} << EOF
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
