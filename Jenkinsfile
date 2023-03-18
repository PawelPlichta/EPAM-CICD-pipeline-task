pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        script {
          git branch: 'main', credentialsId: 'github-id', url: 'https://github.com/PawelPlichta/EPAM-CICD-pipeline-task.git'
          echo 'Git Checkout Completed' 
        }
      }
    }

    stage('Application Build') {
      steps {
        script {
          sh 'chmod +x ./scripts/build.sh'
          sh './scripts/build.sh'
          echo 'Application Build Completed' 
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
          
          sh "docker build -t ${registry}:${BUILD_NUMBER} ."
          sh "docker tag ${registry}:${BUILD_NUMBER} ${registry}:latest "
          echo 'Docker Image Build Completed' 
        }
      }
    }
    stage('Docker Image Push') {
      steps {
        script {
          sh "docker push ${registry}:${BUILD_NUMBER}"  
            echo 'Docker Image Push Completed'
          
        }
      }
    }
    
    
    
  }
  environment {
    registry = 'pawelpl/cicd_task_pawel_plichta'
  }
}
