pipeline {
  environment {
    registry = 'kal1bur/project3'
    registryCredentials = 'docker'
    namespace = 'default'
  }
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/Destiny-s-Child/hmtl.git', branch: 'main')
      }
    }

    stage('Build') {
      steps {
        script {
          dockerImage = docker.build(registry)
        }
      }
    }

    stage('Docker Push') {
      steps {
        script {
          docker.withRegistry('', registryCredentials) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Kubernetes') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
          sh "aws eks update-kubeconfig --region us-east-1 --name teamDC-eksCluster"
          script{
            try{
              sh "kubectl create namespace ${namespace}"
            } catch (Exception e) {
              echo "Exception handled"
            }
          } 
          sh "kubectl apply -f deployment.yaml -n ${namespace}"
          sh "kubectl -n ${namespace} rollout restart deployment teamdc-project"
        }
      }
    }
  }
}