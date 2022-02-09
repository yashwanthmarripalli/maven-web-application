node { //node start

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '8', daysToKeepStr: '', numToKeepStr: '8')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.8.4"

//Get the code from GitHub Repo
stage('CheckoutCode') {

git branch: 'development', credentialsId: '6c835248-7f64-4203-9df8-957aeee9d628', url: 
'https://github.com/yashwanthmarripalli/maven-web-application.git'
}

//Build the code using Maven
stage('Build') {
sh "${mavenHome}/bin/mvn clean package"
}

//Generate the SonarScanner
stage('ExecuteSonarQubeReport') {
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Uploaded Artifacts into Nexus Repo
stage('UploadArtifactInto NexusRepo') {
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer') {
sshagent(['41f221bc-9015-4c0b-8cec-404fb5fb6861']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@http://3.110.223.70:/opt/apache-tomcat-9.0.58/webapps/"

}

}
 

} //node close
