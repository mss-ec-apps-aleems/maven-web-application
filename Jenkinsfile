
node ('master')
{
def mavenHome= tool name: "maven3.8.4"

stage('checkoutCode')
{
git branch: 'development', credentialsId: 'd1baaf81-8f09-4304-9538-7b9a3f9e63eb', url: 'https://github.com/mss-ec-apps-aleems/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat')
{
sshagent(['cfa975a4-4874-48c3-b270-cab6478d4ca3']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.111.57.210:/opt/apache-tomcat-9.0.56/webapps"
}
}

stage('SendNotification')
{
emailext body: '''Build over


Regards
Aleem Shariff''', subject: 'Build Over', to: 'jamalsha55@gmail.com'
}
}
