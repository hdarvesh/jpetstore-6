pipeline {
    agent any
    stages{
        stage('Compile') {
            steps {
               git url: 'https://github.com/hdarvesh/jpetstore-6.git'
               sh 'mvn compile'
              }
        }
		stage("build & SonarQube analysis") 
		{
		steps {
              withSonarQubeEnv('sonarqube') {
                sh 'mvn clean package sonar:sonar'
              }
            }
	    }
        stage('Test') {
           steps {
               sh 'mvn test'
               junit 'target/surefire-reports/*.xml'
           } 
        }
        stage('Publish') 
        {
            steps {
           nexusArtifactUploader artifacts: [[artifactId: 'jpetstore', classifier: '', file: '/var/lib/jenkins/workspace/nexus/target/jpetstore.war', type: 'war']], credentialsId: 'cef7707f-15e0-4865-8aa2-eb090a4c4c9e', groupId: 'org.mybatis', nexusUrl: '23.96.121.78:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'shaheenrepo', version: '6.0.3-SNAPSHOT'
		} 
        }
    }
}
