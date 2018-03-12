pipeline {
    agent {
        docker {
            image 'gradle:4.6' 
            args '-v /root/.gradle:/root/.gradle' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'gradle clean build' 
            }
        }
		stage('Test') {
            steps {
                sh 'gradle clean test'
            }
            post {
                always {
                  step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/build/test-results/TEST-*.xml'
                }
            }
        }
    }
}