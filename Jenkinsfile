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
              //  sh 'scp -o StrictHostKeyChecking=no /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@43.204.150.151:/home/ubuntu/apache-tomcat-9.0.98/webapps/'
            sh 'scp /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@3.110.54.43:/home/ubuntu/apache-tomcat-9.0.98/webapps/'
            }
        }
     /*   stage('running tomcat') {
            steps {
                sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@43.204.150.151
            cd /home/ubuntu/apache-tomcat-9.0.98/bin
            ./startup.sh
            '''
            }
        }
        */
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
