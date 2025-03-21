
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "surendra1302/tomcat"
        IMAGE_TAG = "version2.1"
       
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

        
        stage('Deploy to Container') {
            steps {
                script {
                
                    sh """
                    docker run -d -p 8082:8080 ${DOCKER_HUB_REPO}:${IMAGE_TAG}
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
