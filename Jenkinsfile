pipeline {
    agent {
        label 'agent'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        sh 'mvn test'
                    } catch (Exception e) {
                        echo "Tests failed: ${e.getMessage()}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                // Publish the test results
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo 'Build completed.'
        }
    }
}
