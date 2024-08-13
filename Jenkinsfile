pipeline {
    agent {
        label 'agent'
    }

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                catchError(buildResult: 'UNSTABLE', message: 'Tests failed') {
                    sh 'mvn test'
                }
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }
    }
}
