pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          app = docker.build("matejdocker1/kiii-jenkins")
        }

      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry(REGISTRY, DOCKER_CREDENTIALS) {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // Optionally, you can signal an orchestrator here
          }
        }

      }
    }

  }
  environment {
    REGISTRY = 'https://registry.hub.docker.com'
    DOCKER_CREDENTIALS = 'dockerhub'
  }
  post {
    always {
      echo 'Pipeline completed'
    }

  }
}