  node {
    tools {
        maven 'M3'
    } 
  	stage('Build') {

        println "Sanity Build..."
        bat 'mvn clean compile'
    }

    stage('Deploy to Development') {

      println "Deploying Build to Development Environment..."

      def pom = readMavenPom file: 'pom.xml'
      print "POM artifactId: " + pom.artifactId
      print "Current Build Number: " + currentBuild.number

      def props = readProperties file:'/tmp/jenkins/jobs/esapi/deploy.properties'

      def anypointMuleEnvironment = props['anypoint.mule.environment']
      def anypointUsername = props['anypoint.username']
      def anypointPassword = props['anypoint.password']
      def cloudhubEnv = props['cloudhub.env']
      def cloudhubRegion = props['cloudhub.region']
      def cloudhubWorkerType = props['cloudhub.workerType']
      def cloudhubWorkers = props['cloudhub.workers']
      def cloudhubEnvClientID = props['cloudhub.env.client_id']
      def cloudhubEnvSecretID = props['cloudhub.env.client_secret']
      def applicationName = pom.artifactId + "-dev"

      bat echo "anypointMuleEnvironment=${anypointMuleEnvironment}"
      bat echo "anypointUsername= ${anypointUsername}"
      bat echo "anypointPassword= ${anypointPassword}"
      bat echo "cloudhubEnv= ${cloudhubEnv}"
      bat echo "cloudhubRegion= ${cloudhubRegion}"
      bat echo "cloudhubWorkerType= ${cloudhubWorkerType}"
      bat echo "cloudhubWorkers= ${cloudhubWorkers}"
      bat echo "cloudhubEnvClientID= ${cloudhubEnvClientID}"
      bat echo "cloudhubEnvSecretID= ${cloudhubEnvSecretID}"
      bat echo "applicationName= ${applicationName}"

      withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'M3.6.3',
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        mavenSettingsConfig: '527873a6-b745-4e5d-a996-26e39451845b',
        mavenLocalRepo: '~/.m2/repository') {

        // Run the maven build
        bat """ mvn -e -Dmule.version=4.3.0 -Danypoint.applicationName=${applicationName} -Danypoint.mule.environment=${anypointMuleEnvironment} -Danypoint.username=${anypointUsername} -Danypoint.password=${anypointPassword} -Dcloudhub.env=${cloudhubEnv} -Dcloudhub.region=${cloudhubRegion} -Dcloudhub.workerType=${cloudhubWorkerType} -Dcloudhub.workers=${cloudhubWorkers} -Dcloudhub.env.client_id=${cloudhubEnvClientID} -Dcloudhub.env.client_secret=${cloudhubEnvSecretID} clean install mule:deploy -P cloudhub """

      } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe & FindBugs reports...

    }

  }
