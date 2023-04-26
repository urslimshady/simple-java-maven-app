pipeline {
    agent any
    environment {
        def mvnHome = tool 'maven3'
        def sshUsername = 'shady' // Update with your SSH username
        def sshPassword = 'Rrohit143@!@#$' // Update with your SSH password
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
                // Copy build artifacts to remote server using SCP
                sh "scp -o StrictHostKeyChecking=no target/your-app.jar ${sshUsername}:${sshPassword}@172.190.19.165:/home/simple-maven-app/"

                // Trigger build process on remote server using SSH
                sshagent(credentials: ['app-server-pass']) {
                    sh "sshpass -p '${sshPassword}' ssh -o StrictHostKeyChecking=no ${sshUsername}@172.190.19.165 'cd /home/simple-maven-app; java -jar your-app.jar'"
                }
            }
        }
    }
}
