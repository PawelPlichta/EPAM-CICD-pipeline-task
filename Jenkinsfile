pipeline {
  agent any
  stages {
    stage('Git checkout') {
      steps {
        script {
          git branch: 'main', credentialsId: 'github-id', url: 'https://github.com/PawelPlichta/EPAM-CICD-pipeline-task.git'
        }

      }
    }
    stage('Application build') {
      steps {
        script {
          sh './build.sh'
        }

      }
    }

  }
  environment {
    registry = 'pawelpl/epam-cicd-pipeline-task'
  }
}
