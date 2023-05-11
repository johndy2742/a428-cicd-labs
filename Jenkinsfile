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
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }
        stage('Deploy') { 
            when {
                // will only execute when Manual Approval is approved
                expression { return env.BUILD_USER_ID }
            }
            steps {
                sh './jenkins/scripts/deliver.sh' 
                script {
                    sleep 60 // wait for 1 minute
                }
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
