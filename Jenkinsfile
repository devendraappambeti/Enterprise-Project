pipeline {

agent any

stages {

stage('Checkout') {
steps {
git 'https://github.com/devendraappambeti/azure-devops-enterprise-project.git'
}
}

stage('SonarQube Scan') {
steps {
sh 'sonar-scanner'
}
}

stage('Build Docker Image') {
steps {
sh 'docker build -t devops-app .'
}
}

stage('Security Scan') {
steps {
sh 'trivy image devops-app'
}
}

stage('Push Image') {
steps {
sh 'docker tag devops-app devendra166/devops-app:latest'
sh 'docker push devendra166/devops-app:latest'
}
}

stage('Deploy to Kubernetes') {
steps {
sh 'kubectl apply -f kubernetes/'
}
}

}
}