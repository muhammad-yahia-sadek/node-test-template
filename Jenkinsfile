pipeline {
    agent {
        docker {
            image 'node:16'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }             

    environment {
        // Define environment variables
        DOCKER_REGISTRY = 'mohamedyahiasadek'
        DOCKER_IMAGE = 'nodejs-api-template'
        DOCKER_CREDENTIALS_ID = '9d00d330-6949-40bb-ac8f-dae012c17d96'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from Git repository
                git 'https://github.com/amarthakur0/nodejs-api-template'
            }
        }

        stage('Unit Tests') {
            steps {
                // Run unit tests
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Package the application into a Docker container
                script {
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker container to Docker Hub
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}

