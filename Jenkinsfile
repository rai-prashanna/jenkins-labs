#!groovy
node {

    notifyStarted()
    
   // Mark the code checkout 'stage'....
   stage 'Checkout'
   git url: 'https://github.com/rai-prashanna/jenkins-labs.git'

   //Mark the code build script 'stage'......
   stage 'Find build script'
   sh 'sh game-of-life/build.sh'
   

   // Get the maven tool.
   // ** NOTE: This 'M3' maven tool must be configured
   // **       in the global configuration.           
   def mvnHome = tool 'M3'

   // Mark the code build 'stage'....
   stage 'Build'
   // Run the maven build
   sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore clean package -f game-of-life/pom.xml"
   step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])

   //Mark the code deploy 'stage'
   stage 'deploy'
   sh 'sh game-of-life/deploy.sh'
 
   
 }   
  post {
    always {
      deleteDir()
    }
    success {
      mail to:"prashanna@fusemachines.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
    }
    failure {
      mail to:"prashanna@fusemachines.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
    }
  }
 
