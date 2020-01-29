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
        nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: 'NewJpetRepo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/project pipeline/target/jpetstore.war']], mavenCoordinate: [artifactId: 'jpetstore', groupId: 'org.mybatis', packaging: 'war', version: '9.9']]]
         } 
        }
    }
}
