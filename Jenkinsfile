node
{
    def mavenHome = tool name: "maven3.8.5"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    echo "the job name is: ${env.JOB_NAME}"
    echo "the node name is : ${env.NODE_NAME}"
    echo "the workspace space is: ${env.WORKSPACE}"
    echo "the node label is: ${env.NODE_LABELS}"
    echo "the ebuild number is: ${env.BUILD_NUMBER}"
    try{
        notifyBuild("STARTED")
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: 'e29007c5-c6c8-4b5d-a231-3035740044f0', url: 'https://github.com/sindhuraedara/maven-web-application.git'

    }
    stage('Build'){
    sh "${mavenHome}/bin/mvn clean package"
    }
  /*
    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsIntoNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployAppIntoTomcatServer'){
    sshagent(['7ef183e6-f8ba-4e0b-b3c9-9373c6881000']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.142.222:/opt/apache-tomcat-9.0.64/webapps/"
}
    }
    */
    }
    catch(e){
        currentBuild.result = "FAILURE"
        throw e
    }
    finally{
        notifyBuild(currentBuild.result)
    }
   
}//node closing

def notifyBuild(String buildStatus = 'STARTED') {
  
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'ORANGE'
    colorCode = '#FFA500'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
