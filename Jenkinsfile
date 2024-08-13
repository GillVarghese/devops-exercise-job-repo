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
                    // Run tests and catch errors
                    catchError(buildResult: 'UNSTABLE', message: 'Tests failed') {
                        sh 'mvn test'
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
            // Optional cleanup or notifications
            echo 'Build completed.'
        }
    }
}
