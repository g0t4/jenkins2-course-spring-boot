pipeline {

agent { node { label 'master' } }
    
	stages {
    stage('Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'aecbcb10-5ce2-412c-aef2-7e296a052fbf', url: 'https://github.com/akmaharshi/jenkins2-course-spring-boot.git/']]])
    }
    
    stage('Build') {
        //withMaven(jdk: 'java', maven: 'maven') {
        //maven 'maven'
        //jdk 'java'
        env.PATH = "${tool 'maven'}/bin:${env.PATH}"
        sh 'mvn install -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
      //}
    }
    
    stage('Archive') {
        archive 'spring-boot-samples/spring-boot-sample-atmosphere/target/*.jar'
    }
    
    stage('Test Results') {
        junit allowEmptyResults: true, testResults: 'spring-boot-samples/spring-boot-sample-atmosphere/target/surefire-reports/*.xml'
    }
    
    stage('nexus upload') {
        // sh 'mvn clean deploy -Dmaven.test.skip=true -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
        nexusArtifactUploader {
        nexusVersion('nexus2')
        protocol('http')
        nexusUrl('13.126.209.230:8081/nexus')
        groupId('sp.sd')
        version('1.4.0')
        repository('NexusArtifactUploader')
        credentialsId('442d2cff-ac4d-4143-9f2d-5e89f974f77a')
        artifact {
            artifactId('spring-boot-sample-atmosphere')
            type('jar')
            classifier('debug')
            file('spring-boot-samples/spring-boot-sample-atmosphere/target/spring-boot-sample-atmosphere-1.4.0.BUILD-SNAPSHOT.jar')
        }
    }
	}
    
}
