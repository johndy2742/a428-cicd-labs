node {
    def dockerImage = 'node:16-buster-slim'
    stage('Setup') {
        sh "docker pull $dockerImage"
    }
    stage('Build') {
        sh "docker run -d -p 3000:3000 $dockerImage"
        def containerId = sh(
            script: 'docker ps -lq',
            returnStdout: true
        ).trim()
        sh "docker exec $containerId npm install"
    }
    stage('Test') {
        sh './jenkins/scripts/test.sh'
    }
    stage('Cleanup') {
        sh "docker stop $(docker ps -lq)"
        sh "docker rm -f $(docker ps -lq)"
    }
}
