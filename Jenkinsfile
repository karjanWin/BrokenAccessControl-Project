pipeline {
    agent any
    tools {
        maven 'Maven-BT'  // Ensure Maven is installed in Jenkins
    }
    stages {
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
                // Make sure SonarQube and the scanner are configured correctly in Jenkins
                withSonarQubeEnv('Sonar-Server') {
                    bat "${tool name: 'SonarQubeScanner'}/bin/sonar-scanner"
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
