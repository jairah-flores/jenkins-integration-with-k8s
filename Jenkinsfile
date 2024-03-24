def charts_repo = 'https://github.com/jairahflores/nodeapptest.git'
def target_branch = 'master'
def dockerimagename = "chroot200/nodeapp"
def dockerImage = ""
pipeline {

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
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Push Image to Image Repository') {

      steps{
        script {
          withCredentials([usernamePassword(credentialsId: "dockerhublogin" , passwordVariable: 'password', usernameVariable: 'username')]){
            docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy Application to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }
    
  }

}
