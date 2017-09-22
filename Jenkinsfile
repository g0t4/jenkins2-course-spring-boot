pipeline {
    agent { node { label 'master' } }
	
	environment {
		env.PATH = "${tool 'maven'}/bin:${env.PATH}"
	}
    
 stages {
    stage('Checkout') {
	    steps{
		cleanWs()
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '38bcc0a7-70b9-4fc5-952c-06597c8b66d0', url: 'https://github.com/akmaharshi/jenkins2-course-spring-boot.git/']]])
	    }
    }
	
	    
    stage('Build') {
        //withMaven(jdk: 'java', maven: 'maven') {
        //maven 'maven'
        //jdk 'java'
	    
	    steps{
        
        sh 'mvn package -Dmaven.test.skip=true -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
      }
    }
    
    stage('Archive') {
	    steps{
        archive 'spring-boot-samples/spring-boot-sample-atmosphere/target/*.jar'
	    } }
    
    stage('Test Results') {
	    steps{
        junit allowEmptyResults: true, testResults: 'spring-boot-samples/spring-boot-sample-atmosphere/target/surefire-reports/*.xml'
	    } }
    
    stage('nexus upload') {
        //sh 'mvn clean deploy -Dmaven.test.skip=true'
        steps{
        // sh 'mvn clean deploy -Dmaven.test.skip=true -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
        nexusArtifactUploader artifacts: [[artifactId: 'spring-boot-sample-atmosphere', classifier: '', file: 'spring-boot-samples/spring-boot-sample-atmosphere/target/spring-boot-sample-atmosphere-1.4.0.BUILD-SNAPSHOT.jar', type: 'jar']], credentialsId: 'cf5bbd82-2468-46d3-a99e-13df16ada832', groupId: 'grooup.id', nexusUrl: '13.126.209.230:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'releases', version: '1.4.0'
	}
    }
 
 }
}
