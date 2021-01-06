node ('master')
{
    def mavenHome = tool name: "maven3.6.3"

    stage('github')
    {
        git branch: 'development', credentialsId: 'f4274c6d-aafa-4593-be85-4f095ebf4c30', url: 'https://github.com/amazon-mo/maven-web-application.git'
    }
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('sonarqube')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('nexus')
    {
        sh "${mavenHome}/bin/mvn clean sonar:sonar deploy"
    }
    stage('tomcat')
    {
        sshagent(['ac81ea19-b3c1-49b1-9835-faa8a5a884ae'])
        {
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.183.130.126:/opt/apache-tomcat-9.0.41/webapps/"  
        }
    }
}
