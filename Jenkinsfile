node
{
def mavenHome = tool name: "maven3.6.3"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
stage('Checkoutcode')
{
git branch: 'development', credentialsId: '7ef1d2f4-5253-4ea5-8f3c-b9fc5083bb9a', url: 'https://github.com/Ruhijaba/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('SonarQ report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('Nexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('Deploy app in tomcat')
{
sshagent(['56fcc892-0e12-4b1a-a706-a9b3745cfb27'])
{
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.7.65.83:/opt/apache-tomcat-9.0.41/webapps/"
}
}
stage('email')
{
emailext body: '''Build complete
regards,
Ruhi''', subject: 'Build is over', to: 'ruhijabaroo6@gmail.com,chandanakamatham11@gmail.com'
}
}
