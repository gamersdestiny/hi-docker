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
          /g/applications/docker-windows-amd64.exe compose version
        '''
      }
    }
    stage('Build') {
      steps {
        sh '/g/applications/docker-windows-amd64.exe context use default'
        sh '/g/applications/docker-windows-amd64.exe compose build'
        sh '/g/applications/docker-windows-amd64.exe compose push'
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