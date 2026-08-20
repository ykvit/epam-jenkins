pipeline {
    agent any

    tools {
        nodejs 'node 7.8.0'
    }

    environment {
        PORT = "${BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
        CONTAINER_NAME = "app-${BRANCH_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:v1.0 ."
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    sh "docker run -d --name ${CONTAINER_NAME} --expose ${PORT} -p ${PORT}:3000 ${IMAGE_NAME}:v1.0"
                }
            }
        }
    }
}
