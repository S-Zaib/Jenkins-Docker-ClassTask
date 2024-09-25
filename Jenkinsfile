pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')  // Jenkins credentials ID
        DOCKER_IMAGE = "szaib/ml-app:${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    } else {
                        bat "docker build -t ${DOCKER_IMAGE} ."
                    }
                }
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                script {
                    if (isUnix()) {
                        sh """
                            echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin
                        """
                    } else {
                        bat """
                            echo %DOCKERHUB_CREDENTIALS_PSW% > password.txt
                            docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin < password.txt
                            del password.txt
                        """
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker push ${DOCKER_IMAGE}"
                    } else {
                        bat "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            script {
                if (isUnix()) {
                    sh 'docker logout'
                } else {
                    bat 'docker logout'
                }
            }
        }
    }
}
