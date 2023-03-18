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
          
          sh "docker build -t cicd_task_pawel_plichta:${BUILD_NUMBER} ."
          sh "docker tag cicd_task_pawel_plichta:${BUILD_NUMBER} cicd_task_pawel_plichta:latest "
          
          
        }

      }
    }
      
    
    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-id') {
            docker.image("${registry}:${env.BUILD_NUMBER}").push('latest')
            docker.image("${registry}:${env.BUILD_NUMBER}").push("${env.BUILD_NUMBER}")
          }
        }
    }
  }
  environment {
    registry = 'pawelpl/epam-cicd-pipeline-task'
  }
}
