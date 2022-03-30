pipeline {
parameters {

choice(choices: ['Dev', 'QA'], description: 'Target for build env', name: 'env')

choice(choices: ['Y','N'], description: 'is the deployment for release' , name: 'isRelease')

}

    agent any   
    tools {
    maven 'maven3.8.4'	
	}
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/keyspaceits/javawebapp.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        }
	    stage('S3 Bucket') {
		    steps {
			   withAWS(region:'us-east-1', credentials:'keyuser'){
                    s3Upload(file:'CounterWebApp.war, bucket:'jenkin-qa', path:'/var/lib/jenkins/workspace/mutli-pipeline_main/target')
	    }
			    stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer', path: '', url: 'http://3.231.151.173:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
