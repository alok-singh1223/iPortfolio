pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aa3000/iportfolio"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/alok-singh1223/iportfolio.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t aa3000/iportfolio:%BUILD_NUMBER% .'
            }
        }

        stage('Push Docker Image') {
            steps {
              withCredentials([
                usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )
        ]) {
            bat """
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
            docker push aa3000/iportfolio:%BUILD_NUMBER%
            """
        }
    }
}

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                kubectl set image deployment/iportfolio-deployment ^
                 iportfolio=aa3000/iportfolio:%BUILD_NUMBER%
                '''
            }
        }
    }
}
