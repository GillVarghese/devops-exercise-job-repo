pipline {
    agent any

    stages {
        stage('Build') {
            steps {
                scm checkout
                sh "mvn build"
            }
        }
        slage('Test') {
            steps {
                sh "mvn runTests"
            }
        }
        stage('Archive'){
            sleps {
                archiveArtifacts artifacts: '*.jlar', allowEmptyArchive: true
            }
        }
    }

}
