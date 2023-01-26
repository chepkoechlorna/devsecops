pipeline{
  agent any
  environment {
  PATH = "/opt/maven/bin:$PATH"
  JAVA_HOME = "/usr/lib/jvm/java-1.11.0-openjdk-amd64"
  }
  stages{
    stage('Build'){
      steps{
        sh "mvn clean package"
        sh "mv target/*.war target/myweb.war"
        }
        }
    stage("deploy-dev"){
      steps{
        sshagent(['tomcat-dev1'])
        sh """
          scp -o StrictHostKeyChecking=no target/myweb.war  
          lorna@192.168.100.72:/opt/tomcat/webapps/
          ssh lorna@192.168.100.72 /opt/tomcat/bin/shutdown.sh
          ssh lorna@192.168.100.72 /opt/tomcat/bin/startup.sh
           """
      }
      }
    stage('SonarQube analysis'){
      steps{
        withSonarQubeEnv('sonarqube'){
          sh "mvn sonar:sonar"
          }
          }
          }
          }        
          }
