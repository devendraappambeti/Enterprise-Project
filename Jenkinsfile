pipeline {

agent any

stages {

stage('Checkout') {
steps {
git branch: 'main',
url: 'https://github.com/devendraappambeti/Enterprise-Project.git'
}
}

stage('SonarQube Scan') {
    steps {
        withSonarQubeEnv('SonarQube') {
            bat 'sonar-scanner'
        }
    }
}

stage('Build Docker Image') {
steps {
bat 'docker build -t devops-app .'
}
}

stage('Security Scan') {
steps {
bat 'trivy image devops-app'
}
}

stage('Push Image') {
steps {
bat 'docker tag devops-app devendra166/devops-app:latest'
bat 'docker push devendra166/devops-app:latest'
}
}

stage('Deploy to Kubernetes') {
steps {
bat 'kubectl apply -f kubernetes/'
}
}

}
}
