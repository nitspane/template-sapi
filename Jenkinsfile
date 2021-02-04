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
                echo "Starting Create Release Branch..."
                sh "git checkout -b '${env.BUILD_VERSION}'"
                sh "mvn versions:set -DnewVersion='${env.BUILD_VERSION}'"
                echo "Create Release Branch: ${currentBuild.currentResult}"
            }
            post {
                success {
                    echo "...Create Release Branch Succeeded for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
                } 
                unsuccessful {
                    echo "...Create Release Branch Failed for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
                }
            }
        }   
     
      }
        
    }