def charts_repo = 'https://github.com/jairahflores/nodeapptest.git'
def target_branch = 'master'

pipeline {

  environment {
    dockerimagename = "chroot200/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        script {
        withCredentials([usernamePassword(credentialsId: "github-creds", passwordVariable: 'password', usernameVariable: 'username')]){
            git credentialsId: 'github-creds',
            url: charts_repo
            sh """
                git config remote.origin.url ${charts_repo.replaceAll("//","//${username}:${password}@")}
                git checkout ${target_branch}
            """
          }
        }
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.pull dockerimagename
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

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
