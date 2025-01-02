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
