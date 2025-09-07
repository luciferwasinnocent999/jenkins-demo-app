node {
    def appImage = 'yourdockerhubusername/jenkins-demo-app:latest'

    stage('Checkout') {
        git 'https://github.com/yourusername/jenkins-demo-app.git'
    }

    stage('Build Docker Image') {
        echo "Building Docker image: ${appImage}"
        sh "docker build -t ${appImage} ."
    }

    stage('Push to Docker Hub') {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-creds', 
            usernameVariable: 'DOCKER_USER', 
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
            sh "docker push ${appImage}"
        }
    }

    stage('Run Container') {
        sh "docker rm -f jenkins-demo-app || true"
        sh "docker run -d -p 5000:5000 --name jenkins-demo-app ${appImage}"
    }
}
