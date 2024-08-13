pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }
    }
}
