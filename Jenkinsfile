pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from Git
                git branch: 'master', credentialsId: 'github-creds', url: 'https://github.com/urslimshady/simple-java-maven-app'
            }
        }
        stage('Build') {
            
                // Build the Maven application
		def mvnHome =  tool name: 'maven3'
		steps {

                sh '${mvnHome}/opt/apache-maven-3.9.1/bin/mvn clean install'
            }
            
        }
        stage('Deploy') {
            steps {
                // Copy build artifacts to remote server
                sh 'scp target/your-app.jar shady@172.190.19.165:/home/simple-maven-app/'

                // Trigger build process on remote server
                sshagent(['maven-app-keys']) {
                    sh 'ssh shady@172.190.19.165 "cd /home/simple-maven-app; java -jar your-app.jar"'
                }
            }
        }
    }
}

