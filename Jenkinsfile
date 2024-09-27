pipeline {
    agent any

    environment {
        // Define the custom Maven path located in /opt directory
        MAVEN_HOME = '/opt/maven' // Replace with your actual Maven version
        PATH = "${MAVEN_HOME}/bin:${env.PATH}" // Append Maven to the existing PATH
    }

    stages {
        stage('Checkout') {
            steps {
                // Get the code from the specified branch in your GitHub repository
                git branch: 'main', url: 'https://github.com/MahiTzy/ChatApp-Springboot-WebSocket.git'
            }
        }

        stage('Build and Install') {
            steps {
                // Use custom Maven path for building the project
                sh "${MAVEN_HOME}/bin/mvn clean install"
            }
        }

        stage('SonarQube Code Review') {
            steps {
                // Assuming you have SonarQube configured and available in Jenkins
                withSonarQubeEnv('Sonar-Server-9.9.1') { // Replace with your actual SonarQube configuration name
                    sh "${MAVEN_HOME}/bin/mvn sonar:sonar"
                }
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the application to the specified server
                sshagent(['Tomcat-Server']) {
                    // Disable strict host key checking temporarily
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.111.55.64 "rm -rf /home/ubuntu/tomcat-9/webapps/ChatApp.war"'

                    // Copy the new WAR file to the server
                    sh 'scp -o StrictHostKeyChecking=no target/ChatApp.war ubuntu@3.111.55.64:/home/ubuntu/tomcat-9/webapps/'

                    // Restart Tomcat
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.111.55.64 "/home/ubuntu/tomcat-9/bin/shutdown.sh"'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.111.55.64 "/home/ubuntu/tomcat-9/bin/startup.sh"'
                }
            }
        }
    }
}
