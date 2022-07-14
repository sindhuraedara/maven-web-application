node
{
    def mavenHome = tool name: "maven3.8.5"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    echo "the job name is: ${env.JOB_NAME}"
    echo "the node name is : ${env.NODE_NAME}"
    echo "the workspace space is: ${env.WORKSPACE}"
    echo "the node label is: ${env.NODE_LABELS}"
    echo "the ebuild number is: ${env.BUILD_NUMBER}"
    
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
