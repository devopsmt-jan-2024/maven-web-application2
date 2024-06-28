pipeline{
  
  agent any
  
  tools {
   maven 'maven3.9.6'
  }
  
  stages{
   stage('CheckOutCode'){
   steps{
     sendSlackNotifications("STARTED")
   git branch: 'development', credentialsId: 'bed94d51-4cec-4bf0-83ff-c69b8dfa6209', url: 'https://github.com/devopsmt-jan-2024/maven-web-application2.git'
   }
   }
   
   //Build
   stage('build'){
   steps{
   sh "mvn clean package"
   }
   }
   
   /*
   //Execute SonarQube Report
   stage('SonarQube Report'){
   steps{
   sh "mvn clean sonar:sonar"
   }
   }
   
   //Upload Artifact into Nexus
   stage('Upload Artifacts into Nexus'){
   steps{
   sh "mvn clean deploy"
   }
   }
   
   //Deploy App into Tomcat Server
   stage('deploy app into Tomcat'){
   steps{
   sshagent(['36e9f869-89f3-4366-a08b-2ad85e397ec4']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.9.183:/opt/apache-tomcat-9.0.89/webapps/"
   }
   }
   }
   */
  }//stages closing
  
  post {
  success {
    sendSlackNotifications(currentBuild.result)
  }
  failure {
    sendSlackNotifications(currentBuild.result)
  }
}

  }//pipeline closing
  
  def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
