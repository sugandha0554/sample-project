
pipeline {
    agent any
    environment {
        IMAGE_NAME = 'sugandha05/srinujenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
    }
    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/sugandha0554/sample-project', branch: 'main'
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh "bash -c 'python3 -m venv venv && source venv/bin/activate && pip install --upgrade pip && pip install -r requirements.txt'"
            }
        }

        stage('Test') {
            steps {
                sh "bash -c 'source venv/bin/activate && pytest && whoami'"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    echo "Attempting to log in to Docker Hub..."
                }
                withCredentials([usernamePassword(credentialsId: 'dockerhubpwd', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                }
                echo 'Login successful'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "bash -c 'docker build -t ${IMAGE_TAG} . && echo Docker image built successfully && docker images'"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_TAG}"
                echo "Docker image pushed successfully"
            }
        }
    }
}
