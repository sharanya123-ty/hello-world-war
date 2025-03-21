pipeline {
    agent any

    environment {
        DOCKER_REPO = "sharvibhs/hello-world-war"
        DOCKER_TAG = "latest"
        CONTAINER_NAME = "hello-world-container"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Find Dockerfile') {
            steps {
                script {
                    echo "Searching for Dockerfile..."
                    def dockerfilePath = sh(script: "find . -name 'Dockerfile' | head -n 1", returnStdout: true).trim()
                    
                    if (dockerfilePath) {
                        echo "Dockerfile found at: ${dockerfilePath}"
                        env.DOCKERFILE_PATH = dockerfilePath
                    } else {
                        error "Dockerfile not found in repository!"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image from ${DOCKERFILE_PATH}..."
                sh "docker build -t ${DOCKER_REPO}:${DOCKER_TAG} -f ${DOCKERFILE_PATH} ."
            }
        }

        stage('Login to Docker Hub') {
            environment {
                DOCKER_CREDENTIALS = credentials('docker_hub_token')   // Single credential ID
            }
            steps {
                echo 'Logging in to Docker Hub...'
                sh """
                echo ${DOCKER_CREDENTIALS_PSW} | docker login -u ${DOCKER_CREDENTIALS_USR} --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                sh "docker push ${DOCKER_REPO}:${DOCKER_TAG}"
            }
        }

        stage('Remove Existing Container') {
            steps {
                echo 'Removing old container (if exists)...'
                sh """
                docker rm -f ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh """
                docker run -d --name ${CONTAINER_NAME} -p 8090:8080 ${DOCKER_REPO}:${DOCKER_TAG}
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
        success {
            echo "Container running at http://localhost:8080"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
