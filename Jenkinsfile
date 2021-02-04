pipeline {
    agent any
    environment {
        BUILD_VERSION = "v${currentBuild.number}.RELEASE"
    }
    tools {
        maven 'M3'
    } 
    stages{      
        
        stage('Build and Test') {
            steps {
                //echo "Starting Build and Test..."
                bat "mvn -Dmaven.test.failure.ignore clean verify"
                bat 'echo Build and Test: ${currentBuild.currentResult}'
            }
            post {
                success {
                     bat 'echo Build and Test- Success'
                } 
                unsuccessful {
                      bat 'echo Build and Test- Failed'
                }
            }
        }

		stage('Deploy to Development') {
            
                  def pom = readMavenPom file: 'pom.xml'
				  print "POM artifactId: " + pom.artifactId
				  print "Current Build Number: " + currentBuild.number

				  def props = readProperties file:'deploy.properties'

				  def anypointMuleEnvironment = props['anypoint.mule.environment']
				  def anypointUsername = props['anypoint.username']
				  def anypointPassword = props['anypoint.password']
				  def cloudhubEnv = props['cloudhub.env']
				  def cloudhubRegion = props['cloudhub.region']
				  def cloudhubWorkerType = props['cloudhub.workerType']
				  def cloudhubWorkers = props['cloudhub.workers']
				  def cloudhubEnvClientID = props['cloudhub.env.client_id']
				  def cloudhubEnvSecretID = props['cloudhub.env.client_secret']
				  def applicationName = pom.artifactId
                
                     
            post {
                success {
                    bat 'echo Deploy Success'
                } 
                unsuccessful {
                    bat 'echo Deploy to Development Failed'
                    
                }
            }
        }
     
      }
        
    }