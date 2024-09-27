
pipeline {
  agent any
  stages {
     stage('Checkout') {
            steps {
               checkout scm
            }
        }
    stage('Docker Build') {
      steps {
        sh 'docker build -t honeyy02/weather-app .'
      }
    }
    stage('Docker Run') {
      steps {
        sh 'docker run -d honeyy02/weather-app'
      }
    }
    stage('Docker Login') {
      steps {
       withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')])  {
          sh 'echo $DOCKERHUB_PASSWORD | docker login -u honeyy02 --password-stdin'
      }
    }
    }
    stage('Docker Push') {
      steps {
        sh 'docker push honeyy02/weather-app'
      }
    }
    stage('Deployment') {
      steps {
        sh 'kubectl apply -f deployment.yaml --validate=false'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }
}
