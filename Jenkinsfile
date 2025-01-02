pipeline {
    agent none
    stages {
        stage('checkout') {
             agent { label 'maven' }
            steps {
                sh 'rm -rf hello-world-war'
                sh 'git clone https://github.com/surendra1302/hello-world-war.git'
            }
        }
        stage('build') {
            agent { label 'maven' }
            steps {
               sh 'cd hello-world-war'
                sh 'mvn clean package'
            }
        }
         stage('deploy') {
              agent { label 'maven' }
            steps {
                sh 'scp -o StrictHostKeyChecking=no /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@13.235.77.147:/home/ubuntu/apache-tomcat-9.0.98/webapps/'
            }
        }
        stage('running tomcat') {
            agent { label 'maven' }
            steps {
                sh 'ssh ubuntu@13.235.77.147 -y'
               sh 'cd /home/ubuntu/apache-tomcat-9.0.98/bin/'
                sh './startup.sh'
            }
        }
    }
    post {
        success {
            emailext(
                subject: "Build success",
                body: "Build is successful",
                to: 'surendrakorivi96@gmail.com'
            )
        }
    failure {
            emailext(
                subject: "Build failed",
                body: "Build was failed ",
                to: 'surendrakorivi96@gmail.com'
            )
        }
    }
}
