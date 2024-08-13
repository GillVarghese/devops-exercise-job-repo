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
                script {
                    try {
                        sh 'mvn test'
                    } catch (Exception e) {
                        // Handle test failures
                        currentBuild.result = 'UNSTABLE'
                        echo 'Tests failed. Marking build as UNSTABLE.'
                    }
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
