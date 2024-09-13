pipeline {

  environment {
    dockerimagename = "venketdr/venketk8"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
/*
    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes") 
        }
      }
    }

  }
*/
    stage('Apply Kubernetes files') {
      steps {
        script {
          withKubeConfig([credentialsId: 'kubernetes', serverUrl: 'https://172.31.10.14:6443']) {
            sh 'kubectl delete pods --all'
            sh 'kubectl apply -f deploymentservice.yml'
          }
        }
      }
    }
  }
}
