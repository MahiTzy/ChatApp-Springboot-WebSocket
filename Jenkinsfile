pipeline {
    agent any

    environment {
        // Define the custom Maven path located in /opt directory
        MAVEN_HOME = "/opt/maven" // Replace with your actual Maven version
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
    }
}
