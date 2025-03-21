
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "surendra1302/tomcat"
        IMAGE_TAG = "version2.1"
        EC2_USER = "ubuntu"   
        EC2_HOST = "52.90.14.125"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/surendra1302/hello-world-war.git'
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

        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_PRIVATE_KEY', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                    #ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} << EOF
                    ssh -o StrictHostKeyChecking=no ubuntu@52.90.14.125 << EOF
                    docker pull ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                    docker stop tomcat || true
                    docker rm tomcat || true
                    docker run -d -p 8082:8080 --name tomcat ${DOCKER_HUB_REPO}:${IMAGE_TAG}
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
