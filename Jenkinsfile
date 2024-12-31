pipeline {
    parameters {
        string(name: 'cmd', defaultValue: 'package', description: 'building')
        choice(name: 'ch', choices: ['package', 'install', 'deploy'], description: 'Pick something')    
    }
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
                sh 'mvn clean $cmd'
            }
        }
    }
}
