node{
    
    def maven_path = tool name: "maven 3.6.3"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('CodeCheckOut'){
        
        
        git credentialsId: '80b47b1f-b688-411a-9db0-4fc30116bac9', url: 'https://github.com/Chandrakanthbatta/maven-web-application.git'
        
    }
    
    stage('BuildProject'){
        
        sh "${maven_path}/bin/mvn clean package"
        
    }
    
    stage('executesonarqubereport'){
        
         sh "${maven_path}/bin/mvn sonar:sonar"
        
        }
    
    stage('deployartifacttonexus'){
        
         sh "${maven_path}/bin/mvn deploy"
    }
    
    stage('deploywarfiletotomcat'){
        
        sshagent(['2d0d3218-4e0c-4f22-b858-4f0ae68b7ad7']) {
            
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.174.144.12:/opt/tomcat/webapps/"
        }
    }
    
    stage('sendemailnotification'){
        
        emailext body: '''Build $BUILD_NUMBER is success

Regards
Chandrakanth''', subject: 'Build success', to: 'chandrakanthbatta.devops@gmail.com'
        
    }
    
}
