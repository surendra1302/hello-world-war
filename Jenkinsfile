pipeline {
    parameters {
        string(name: 'cmd', defaultValue: 'package', description: 'building')
        choice(name: 'ch', choices: ['package', 'install', 'deploy'], description: 'Pick something')    
    }
    agent any
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
                sh 'mvn clean $cmd'
            }
        }
    }
}
