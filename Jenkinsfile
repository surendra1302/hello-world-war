pipeline {
    agent any
       stages 
    {
        stage('checkout') {             
            steps {
                sh """
                #!/bin/bash
                sudo su
                cd /home/ubuntu/apache-tomcat-10.1.34/webapps
                ls
                curl -L -u "env.ARTIFACTORY_USERNAME:env.ARTIFACTORY_API_KEY" -O "http://13.127.219.52:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.0.${env.GITHUB_RUN_NUMBER}/hello-world-war-1.0.0.${env.GITHUB_RUN_NUMBER}.war"
                pwd
                cd /home/ubuntu/apache-tomcat-10.1.34/bin
                ./shutdown.sh
                cd /home/ubuntu/apache-tomcat-10.1.34/bin
                ./startup.sh
                """ 
            }
        }
         
    }
}
