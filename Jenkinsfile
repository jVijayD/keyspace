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
		
                        s3Upload acl: 'Private', bucket: 'jenkin-qa', file: 'CounterWebApp.war', path:'s3://jenkin-qa/'
	
		    }
	    }
   }
}
