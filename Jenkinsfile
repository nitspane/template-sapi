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
   
        
            post {
                success {
                    echo "...Deploy to Development Succeeded for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
                } 
                unsuccessful {
                    echo "...Deploy to Development Failed for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
                    //script {
        	            //dropLocalReleaseBranch()
                        // Drop Remote Branch
                        // Undeploy from Artifact Management Repo
                        // Rollback to previous version
                   // }
                }
            }
        }
        
    }

   post {
       success {
           echo "All Good: ${env.BUILD_VERSION}"
       }
       unsuccessful {
           echo "Not So Good: ${env.BUILD_VERSION}"
       }      
       always {
           echo "Pipeline result: ${currentBuild.result}"
           echo "Pipeline currentResult: ${currentBuild.currentResult}"
       }
   }
   
}

void dropLocalReleaseBranch() {
    echo "Starting Drop Local Release Branch: ${env.BUILD_VERSION} ..."
    sh "git checkout develop"
    sh "git branch -d '${env.BUILD_VERSION}'"
    echo "...Local Release Branch ${env.BUILD_VERSION} Dropped"
}