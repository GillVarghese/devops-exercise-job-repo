pipeline {
    agent {
        label 'agent'
    }

    options {
        // Retain the build artifacts and clean the workspace manually
        buildDiscarder(logRotator(numToKeepStr: '10')) // Keep 10 builds
        skipStagesAfterUnstable() // Skip stages after an unstable result
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
                // Run tests and mark build as unstable if tests fail
                catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                    sh 'mvn test'
                }
                // Publish test results to Jenkins
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Coverage') {
            steps {
                // Calculate and publish test coverage
                sh 'mvn jacoco:report'
                jacoco()
            }
        }
        stage('Archive') {
            steps {
                // Archive build artifacts
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Clean the workspace after the build
            deleteDir() 
        }
    }

    triggers {
        // Example triggers
        pollSCM('* * * * *') // Poll SCM every minute. Adjust as needed
        // For GitHub pull requests, use the GitHub Branch Source plugin or configure GitHub Webhooks
    }
}
