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
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', submitterParameter: 'APPROVER'
            }
        }
        stage('Deploy') { 
            when {
                expression { params.APPROVER == 'Proceed' }
            }
            steps {
                sh './jenkins/scripts/deliver.sh' 
                script {
                    sleep 60 // menunggu 1 menit
                }
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
