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
                sh 'scp /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@13.235.77.147:/home/ubuntu/apache-tomcat-9.0.98/webapps/'
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
