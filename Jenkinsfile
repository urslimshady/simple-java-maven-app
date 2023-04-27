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
        sshagent(credentials: ['application-server-creds']) {
            // Use scp command to copy the JAR file to the remote server
            sh "scp target/your-app.jar azureuser@52.136.127.164:/home/simple-maven-app/"
        }

        // Trigger build process on remote server using IP address and username/password
        sshagent(credentials: ['application-server-creds']) {
            sh 'ssh azureuser@52.136.127.164 "cd /home/simple-maven-app; java -jar your-app.jar"'
        }
    }
}

    }
}
