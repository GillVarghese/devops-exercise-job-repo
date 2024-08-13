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
                    // Catch errors from test stage, mark build as UNSTABLE if tests fail
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

        stage('Publish Code Coverage') {
            steps {
                // Publish JaCoCo coverage report
                jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/target/classes', sourcePattern: '**/src/main/java', exclusionPattern: '**/target/test-classes'
            }
        }

        stage('Archive') {
            steps {
                // Archive the build artifacts
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
