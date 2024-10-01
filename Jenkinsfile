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
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
