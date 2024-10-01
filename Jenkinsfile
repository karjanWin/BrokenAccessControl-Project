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
    tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    withSonarQubeEnv('SonarQube') {
        bat 'sonar-scanner'  // Make sure to configure SonarQube in Jenkins
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat "${scannerHome}/bin/sonar-scanner"
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
