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
            steps {
                // Build the Maven application
                sh 'mvn clean install'
            }
        }
        stage('Deploy') {
            steps {
                // Copy build artifacts to remote server
                sh 'scp target/your-app.jar root@172.190.19.165:/home/simple-maven-app/'

                // Trigger build process on remote server
                sshagent(['secret.key']) {
                    sh 'ssh root@172.190.19.165 "cd /home/simple-maven-app; java -jar your-app.jar"'
                }
            }
        }
    }
}

