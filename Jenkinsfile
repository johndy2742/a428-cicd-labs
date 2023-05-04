node {
    def dockerImage = 'node:16-buster-slim'
    stage('Setup') {
        sh "docker pull $dockerImage"
    }
    stage('Build') {
        sh "docker run -d -p 3000:3000 $dockerImage"
        sh 'docker exec $(docker ps -lq) npm install'
    }
    stage('Test') {
        sh './jenkins/scripts/test.sh'
    }
    post {
        always {
            sh "docker stop $(docker ps -lq)"
            sh "docker rm -f $(docker ps -lq)"
        }
    }
}
