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
                sh 'sudo scp -r /home/slave1/workspace/maven_job/target/hello-world-war-1.0.0.war ubuntu@13.233.77.156:/opt/apache-tomcat-9.0.98/webapps/'
            }
        }
    }
post {
        success {
            emailext(
                subject: "Build Successful",
                body: """<p>Good news! The build was successful.</p>""",
                to: 'surendrakorivi96@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "Build Failed",
                body: """<p>Unfortunately, the build failed.</p>""",
                to: 'surendrakorivi96@gmail.com'
            )
        }
      }
   }
