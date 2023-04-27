pipeline {
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
        sh 'docker build -t kal1bur/project3 .'
      }
    }

    stage('Docker Login') {
      steps {
        sh 'docker login -u kal1bur -p dckr_pat_DASZeW9LwGJhy0nqeeQtHLUekRI'
      }
    }

    stage('Docker Push') {
      steps {
        sh 'docker push kal1bur/project3'
      }
    }

  }
}