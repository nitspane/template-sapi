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
            steps {
                script {
                    //echo "Starting Deploy to Development"
                        
                    applicationName = template-sapi
                    //echo "applicationName=${applicationName}"
                    
                    props = readProperties(file: '/tmp/Jenkins/esapi/deploy.properties')
                    anypointMuleVersion = '4.3.0'
                    anypointMuleEnvironment = 'SANDBOX'
                    anypointUsername = 'np_mule1'
                    anypointPassword = 'Niharika123*'
                    cloudhubEnv = 'SANDBOX'
                    cloudhubRegion = 'us-east-2'
                    cloudhubWorkerType = 'MICRO'
                    cloudhubWorkers = '1'
                    cloudhubEnvClientID = '7766b70042084c3580f6472d8d6d9213'
                    cloudhubEnvSecretID = '2Ce466f2dBeA4f47a7cb2A0A8D76bc4f'     
                
                        
                    //echo "Deploy to Development: ${currentBuild.currentResult}"
                }
                
                  configFileProvider([configFile(fileId: '527873a6-b745-4e5d-a996-26e39451845b', variable: 'MAVEN_SETTINGS_XML')]) {
                    // Run the maven build
                    bat """ mvn -U --batch-mode -s $MAVEN_SETTINGS_XML \
                        -Dmule.version=${anypointMuleVersion}  \
                        -Danypoint.applicationName=${applicationName} \
                        -Danypoint.mule.environment=${anypointMuleEnvironment} \
                        -Danypoint.username=${anypointUsername} \
                        -Danypoint.password=${anypointPassword} \
                        -Dcloudhub.env=${cloudhubEnv} \
                        -Dcloudhub.region=${cloudhubRegion} \
                        -Dcloudhub.workerType=${cloudhubWorkerType} \
                        -Dcloudhub.workers=${cloudhubWorkers} \
                        -Dcloudhub.env.client_id=${cloudhubEnvClientID} \
                        -Dcloudhub.env.client_secret=${cloudhubEnvSecretID} \
                        clean install mule:deploy -P cloudhub """
                }
            }
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