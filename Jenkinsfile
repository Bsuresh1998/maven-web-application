node{
    def mavenHome = tool name: "maven3.8.2"
    echo "GitHub BranchName ${env.BRANCH_NAME}"
    echo "JENKINS JOBNumber ${env.BUILD_NUMBER}"
    echo  "Jenkins Nodenumbeer ${env.NODE_NAME}"
    echo "JENKINS Home ${env.JENKINS_HOME}"
    echo "JENKINS URL ${env.JENKINS_URL}"
    echo "JOB Name ${env.JOB_Name}"
    stage('CheckOutCode'){
        git branch: 'development', credentialsId: 'b828b692-3259-4597-8834-99e5fba43505', url: 'https://github.com/Bsuresh1998/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('sonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    stage('UploadArtifactIntoNexus'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage('DeployAppIntoTomcatserver'){
        sshagent(['245d23b3-d2d8-4af8-a8a7-f23abe0eb72a']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.180.181:/opt/apache-tomcat-9.0.52/webapps"
    }
    }
    
    stage('SendEmailNotification'){
        emailext body: '''build is over 
regards
suresh''', subject: 'Build is over####@@', to: 'sureshbodisetty1998@gmail.com'
        
    }
}
