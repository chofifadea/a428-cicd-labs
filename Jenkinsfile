pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Waiting for Approval'){
            steps{
                timeout(time: 1, unit: "MINUTES") {
                input message: 'Are you ready for deploy?', submitter: 'dicoding'
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sleep 60
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
