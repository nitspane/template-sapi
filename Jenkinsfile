pipeline {
    agent any
    environment {
        BUILD_VERSION = "v${currentBuild.number}.RELEASE"
    }
    tools {
        maven 'M3'
    } 
    stages{
        
        stage('Create Release Branch') {
            steps {
               // echo "Starting Create Release Branch..."
                bat "git checkout -b '${env.BUILD_VERSION}'"
                bat "mvn versions:set -DnewVersion='${env.BUILD_VERSION}'"
             //   echo "Create Release Branch: ${currentBuild.currentResult}"
            }
            post {
                success {
                   bat 'echo Create Release Branch Succeeded'
                } 
                unsuccessful {
                   bat 'echo Create Release Branch Failed'
                }
            }
        }   
     
      }
        
    }