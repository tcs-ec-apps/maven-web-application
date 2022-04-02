node
{
    def mavenHome = tool name: "3.6.3"
 stage('CheckoutCode')
 {
 git branch: 'development', credentialsId: '09f4816d-1171-4cda-ae3c-758fc790f92f', url: 'https://github.com/tcs-ec-apps/maven-web-application.git'
 }

 stage('Build')
 {
 sh "${mavenHome}/bin/mvn clean package"
 }
 
 stage('ExcuteSonarqubeReport')
 {
     sh "${mavenHome}/bin/mvn sonar:sonar"
 }
 
  stage('UploadArtifactIntoNexus')
 {
 sh "${mavenHome}/bin/mvn deploy"
 }
  stage('DeployAppIntoTomcat')
 {
 sshagent(['baef7a34-5936-4f3e-827e-b18bda8e3a67'])
 {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@18.219.52.4:/opt/apache-tomcat-9.0.58/webapps"
 }
 }
 
 Stage('SendEmailNotification')
 {
     mail bcc: '', body: '''Hi,

Build Over.

Regards,
Praveen''', cc: 'praveenpvp406@gmail.com', from: '', replyTo: '', subject: 'Build Over', to: 'praveenpvp406@gmail.com'
 }
}
