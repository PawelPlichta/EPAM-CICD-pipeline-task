pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        script {
          git branch: 'main', credentialsId: 'github-id', url: 'https://github.com/PawelPlichta/EPAM-CICD-pipeline-task.git'
        }

      }
    }
    stage('Application Build') {
      steps {
        script {
          sh 'chmod +x ./scripts/build.sh'
          sh './scripts/build.sh'
        }

      }
    }
    stage('Tests') {
      steps {
        script {
          sh 'chmod +x ./scripts/test.sh'
          sh './scripts/test.sh'
        }

      }
    }
    stage('Docker Image Build') {
      steps {
        script {
          checkout scm
          sh "docker build -t CICD_practical_task_Pawel_Plichta:${BUILD_NUMBER}"
          sh "docker tag CICD_practical_task_Pawel_Plichta:${BUILD_NUMBER} CICD_practical_task_Pawel_Plichta:latest"
          
        }

      }
    }
  }
  environment {
    registry = 'pawelpl/epam-cicd-pipeline-task'
  }
}
