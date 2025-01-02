pipeline {
    agent { label 'maven' }
    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf hello-world-war'
                sh 'git clone https://github.com/surendra1302/hello-world-war.git'
            }
        }
        stage('build') {
            steps {
               sh 'cd hello-world-war'
                sh 'mvn clean package'
            }
        }
         stage('deploy') {
            steps {
                sh 'scp -r /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@13.233.77.156:/home/ubuntu/apache-tomcat-9.0.98/webapps/'
            }
        }
    }
post {
        success {
              mail to:'surendrakorivi96@gmail.com',
                subject:"Build Successful",
                body:"The build was successful".
                
        failure {
              mail to:'surendrakorivi96@gmail.com',
                subject:"Build Failed",
                body:"The build has Failed".
                
    }
}
