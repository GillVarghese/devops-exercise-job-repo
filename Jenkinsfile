pipeline {
    agent {
        label 'agent'
    }

    options {
        // Keep the Jenkins workspace clean and keep the build history
        cleanWs() 
        // Retain the build artifacts for a longer time
        buildDiscarder(logRotator(numToKeepStr: '10')) 
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

    triggers {
        // Trigger builds for every branch and pull requests
        pollSCM('* * * * *') // Poll SCM every minute
        githubPullRequest {
            triggerPhrase('Jenkins please test this')
            useGitHubHooks()
        }
    }
}
