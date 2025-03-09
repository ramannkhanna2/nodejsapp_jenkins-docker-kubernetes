pipeline {

  environment {
    dockerimagename = "ramann123/myimage"
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
  }
} 



    stage('Apply Kubernetes files') {
      steps {
        script {
          withKubeConfig([credentialsId: 'kubernetes', serverUrl: 'https://172.31.10.224:6443']) {
            sh 'kubectl delete pods --all'
            sh 'kubectl apply -f deploymentservice.yml'
          }
        }
      }
    }
  }
}

