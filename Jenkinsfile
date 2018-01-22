#!groovy
node {

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
  
   try {
     sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore clean package -f game-of-life/pom.xml"
     step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
  }
  catch(all){
      if(currentBuild.result!='FAILURE'){
         emailext body: '$DEFAULT_CONTENT', subject: '$BUILD_STATUS - $JOB_NAME', to: 'prashanna@fusemachines.com'
      }
  }

   //Mark the code deploy 'stage'
   stage 'deploy'
   sh 'sh game-of-life/deploy.sh'
 
   
 }   
  
 
