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
        withAWS(credentials: "aws") {
          sh 'kubectl apply -f deployment/deployment.yaml'
	        sh 'kubectl apply -f deployment/load-balancer.yaml' 
	        sh 'kubectl get svc'
	        sh 'kubectl get pods'
        }
      }
    }
    stage('Clean Up') {
      steps {
        sh 'docker system prune'
      }
    }
  }
}
