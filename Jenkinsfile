pipeline {
    agent any
    options {
        skipDefaultCheckout()  // This avoids conflicts during checkout
    }
    tools {
        maven 'Maven-BT'  // Ensure Maven is installed in Jenkins
    }
    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()  // Deletes the workspace directory before running
            }
        }
        stage('Clone Repository') {
            steps {
                git 'https://github.com/karjanWin/BrokenAccessControl-Project.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Run the tests using Maven. JUnit tests are typically run in this phase.
                bat 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Ensure SonarQube is configured with this name in Jenkins
                withSonarQubeEnv('Sonar-Server') {
                    bat 'sonar-scanner'  // Use `${scannerHome}/bin/sonar-scanner` if needed
                }
            }
        }
        stage('Deploy') {
            steps {
                bat 'mvn clean package'
            }
        }
    }
    post {
        always {
            // Save the JAR file and test reports as artifacts
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit 'target/surefire-reports/*.xml'  // Archive the test results for JUnit
        }
    }
}
