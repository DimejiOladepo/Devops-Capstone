pipeline {
  environment {
    registry = "dimeji/capstone-app"
    registryCredential = "dockerhub"
  }
  agent any
  stages {
    stage('build') {
      steps {
        sh 'echo Building...'
      }
    }
    stage('lint') {
      steps {
        sh 'tidy -q -e index.html'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t dimeji/capstone-app:v1 .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withDockerRegistry([url: "", credentialsId: "dockerhub"]) {
          sh 'docker push dimeji/capstone-app:v1'
        }
      }
    }
    stage('Deployment') {
      steps {
        sh 'echo hurrah'
      }
    }
    stage('Clean Up') {
      steps {
        sh 'docker system prune'
      }
    }
  }
}
