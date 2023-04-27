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
        // Copy build artifacts to remote server using IP address and username/password
       withCredentials([usernamePassword(credentialsId: 'app-server-pass', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            // Use scp command to copy the JAR file to the remote server
            sh "scp -o StrictHostKeyChecking=no target/your-app.jar ${USERNAME}:${PASSWORD}@172.190.19.165:/home/simple-maven-app/"
        }

        // Trigger build process on remote server using IP address and username/password
        sshagent(credentials: ['app-server-pass']) {
            sh 'ssh -o StrictHostKeyChecking=no -l shady -o PubkeyAuthentication=no -o PasswordAuthentication=yes 172.190.19.165 "cd /home/simple-maven-app; java -jar your-app.jar"'
        }
    }
}

    }
}
