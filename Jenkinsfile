pipeline {

agent any

tools {
    sonarQube 'sonar-scanner'
}

environment {
    IMAGE_NAME = "devendra166/devops-app"
    IMAGE_TAG = "latest"
    SONARQUBE_ENV = "SonarQubeServer"
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'main',
            url: 'https://github.com/devendraappambeti/Enterprise-Project.git'
        }
    }

    stage('SonarQube Scan') {
        steps {
            withSonarQubeEnv("${SONARQUBE_ENV}") {
                bat "${tool 'sonar-scanner'}\\bin\\sonar-scanner.bat"
            }
        }
    }

    stage('Quality Gate') {
        steps {
            timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }

    stage('Build Docker Image') {
        steps {
            bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
        }
    }

    stage('Security Scan') {
        steps {
            bat "trivy image %IMAGE_NAME%:%IMAGE_TAG%"
        }
    }

    stage('Docker Login') {
        steps {
            withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                bat "echo %DOCKER_PASS% | docker login -u devendra166 --password-stdin"
            }
        }
    }

    stage('Push Image') {
        steps {
            bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
            bat "kubectl apply -f kubernetes/"
        }
    }
}

}
