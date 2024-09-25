pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_IMAGE = "your-dockerhub-username/ml-app:${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                sh "docker push ${DOCKER_IMAGE}"
            }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}