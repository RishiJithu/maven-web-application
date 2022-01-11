node{
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])
    
    def mavenHome = tool name: "maven3.8.4"
    stage("Check Out Code"){
        git branch: 'development', credentialsId: '95a40599-709e-4f8d-af80-af5b7fb4cb1c', url: 'https://github.com/RishiJithu/maven-web-application.git'
    }
    
    stage("Build"){
    sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("SonarQube Report"){
    sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    
    stage("Deploy Artifacts to Nexus"){
    sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage("Deploy war files to Tomcat"){
        sshagent(['d29e22cd-debd-42c3-a442-0e308b2c3ae2']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.239.68:/opt/apache-tomcat-9.0.56/webapps/"
}
   
    }
    
}
