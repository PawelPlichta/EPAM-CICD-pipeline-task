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
        post {
         success {
            echo 'Tests check passed'
         }
         failure {
            echo 'Tests check failed'
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
    
    stage('Scan Docker Image for Vulnerabilities') {
      steps {
        script {
          sh 'mkdir -p reports'
          sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'
          def vulnerabilities = sh(script: "trivy image --ignore-unfixed --exit-code 0 --severity CRITICAL,HIGH,MEDIUM --format template --template '@html.tpl' -o reports/image-scan.html --no-progress ${registry}:${env.BUILD_ID}", returnStdout: true).trim()
          echo "Vulnerability Report:\n${vulnerabilities}"
          publishHTML target : [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'image-scan.html',
                    reportName: 'Trivy Scan',
                    reportTitles: 'Trivy Scan'
                ]
         }
      }
    }
        
    stage('Docker Image Push') {
      steps {
        script {
          
          docker.withRegistry('', 'dockerhub_id') {
          
            docker.image("${registry}:${env.BUILD_NUMBER}").push('latest')
            docker.image("${registry}:${env.BUILD_NUMBER}").push("${env.BUILD_NUMBER}")
            echo 'Docker Image Push to DockerHub Completed' 
            
          }
          
        }
      }
    }
    
    
    
  }
  environment {
    registry = 'pawelpl/cicd_task_pawel_plichta'
  }
}
