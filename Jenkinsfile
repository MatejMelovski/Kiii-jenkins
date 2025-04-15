pipeline {
    agent any  // This allows Jenkins to run on any available agent

    environment {
        REGISTRY = 'https://registry.hub.docker.com'
        DOCKER_CREDENTIALS = 'dockerhub'  // The Jenkins credentials ID for DockerHub login
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm  // This clones the repository based on the SCM configuration
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build the Docker image
                    app = docker.build("matejdocker1/Kiii-jenkins")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // Push the Docker image to DockerHub
                    docker.withRegistry(REGISTRY, DOCKER_CREDENTIALS) {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                        // Optionally, you can signal an orchestrator here
                    }
                }
            }
        }
    }

    post {
        always {
            // Optional cleanup actions or notifications can go here
        }
    }
}
