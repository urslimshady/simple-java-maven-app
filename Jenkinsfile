pipeline {
    agent any
    environment {
	def mvnHome = tool 'maven3'
	}
    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from Git
                git branch: 'master', credentialsId: 'github-creds', url: 'https://github.com/urslimshady/simple-java-maven-app'
            }
        }
        stage('Build') {
            // Build the Maven application
            steps {
                    sh "${mvnHome}/bin/mvn clean install"                
            }
        }
        stage('Deploy') {
            steps {
                // Copy build artifacts to remote server
                sh 'scp -o StrictHostKeyChecking=no target/your-app.jar shady@172.190.19.165:/home/simple-maven-app/'

                // Trigger build process on remote server
                sshagent(['maven-app-keys']) {
                    sh 'ssh -o StrictHostKeyChecking=no shady@172.190.19.165 "cd /home/simple-maven-app; java -jar your-app.jar"'
                }
            }
        }
    }
}

