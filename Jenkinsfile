pipeline {
  agent 'any'
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKER_HUB_CREDS = credentials('docker-hub-creds')
    AWS_CREDS = credentials('jenkins-ecs-user')
  }
  stages {
    stage('Tooling versions') {
      steps {
        sh '''
          docker --version
          docker compose version
        '''
      }
    }
    stage('Build') {
      steps {
        sh 'docker context use default'
        sh 'docker ecs compose build'
        sh 'docker ecs compose push'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker context use myecscontext'
        sh 'docker compose up'
        sh 'docker compose ps --format json'
      }
    }
  }
}