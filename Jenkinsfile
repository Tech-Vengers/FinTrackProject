pipeline {
  agent any
  environment {
    APP_NAME = "fintrack_app"
    CONTAINER_NAME = "${APP_NAME}_container"
    APP_PORT = "5001"
    IMAGE_TAG = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Clean Workspace') {
      steps { cleanWs() }
    }
    stage('Clone Code') {
      steps {
        git branch: 'main',
        url: 'https://github.com/Tech-Vengers/FinTrackProject.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh "docker build --no-cache -t ${APP_NAME}:${IMAGE_TAG} ."
      }
    }
    stage('Stop & Remove Existing Container') {
      steps {
        sh """
        docker stop ${CONTAINER_NAME} || true
        docker rm ${CONTAINER_NAME} || true
        """
      }
    }
    stage('Run Docker Container') {
      steps {
        sh "docker run -d -p ${APP_PORT}:${APP_PORT} --name ${CONTAINER_NAME} ${APP_NAME}:${IMAGE_TAG}"
      }
    }
  }
}
