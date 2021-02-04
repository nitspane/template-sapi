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
               // bat "git checkout -b '${env.BUILD_VERSION}'"
              //  bat "mvn versions:set -DnewVersion='${env.BUILD_VERSION}'"
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
                    anypointMuleVersion = 4.3.0
                    anypointMuleEnvironment = SandBox
                    anypointUsername = np_mule1
                    anypointPassword = Niharika123*
                    cloudhubEnv = props['cloudhub.env']
                    cloudhubRegion = us-east-2
                    cloudhubWorkerType = MICRO
                    cloudhubWorkers = 1
                    cloudhubEnvClientID = 7766b70042084c3580f6472d8d6d9213
                    cloudhubEnvSecretID = 2Ce466f2dBeA4f47a7cb2A0A8D76bc4f     
                
                        
                    //echo "Deploy to Development: ${currentBuild.currentResult}"
                }
                
                configFileProvider([configFile(fileId: '107138b1-fdbe-45c5-8eb3-64a520257c07', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS_XML')]) {
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