
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "your-dockerhub-username/my-tomcat-app"
        IMAGE_TAG = "latest"
        EC2_USER = "ec2-user"   // Use "ubuntu" for Ubuntu AMIs
        EC2_HOST = "your-ec2-public-ip"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/java-tomcat-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKER_HUB_REPO}:${IMAGE_TAG} .
                """
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh """
                    echo "${DOCKER_HUB_PASSWORD}" | docker login -u "${DOCKER_HUB_USERNAME}" --password-stdin
                    docker push ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Pull Image After Push') {
            steps {
                sh """
                docker pull ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                """
            }
        }

        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_PRIVATE_KEY', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} << EOF
                    docker pull ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                    docker stop my-tomcat-app || true
                    docker rm my-tomcat-app || true
                    docker run -d -p 8080:8080 --name my-tomcat-app ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
