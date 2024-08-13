pipeline {
    agent agent

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh "mvn build"
            }
        }
        stage('Test') {
            steps {
                sh "mvn runTests"
            }
        }
        stage('Archive'){
            steps {
                archiveArtifacts artifacts: '*.jar', allowEmptyArchive: true
            }
        }
    }
}
