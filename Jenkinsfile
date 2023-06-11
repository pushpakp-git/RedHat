pipeline {
	agent{
	label 'slave2'
	}
	parameters {
		choice(name: 'ENVIRONMENT', choices: ['QA','UAT'], description: 'Pick Environment value')
	}
	stages {
	    stage('Checkout') {
	        steps {
			checkout scm			       
		      }}
		stage('Build') {
	           steps {
			  sh '/home/dev/apache-maven-3.9.1/bin/mvn install'
	                 }}
		stage('Deployment'){
		    steps {
			sh 'sshpass -p "dev" scp target/RedHat.war dev@172.17.0.3:/home/dev/apache-tomcat-9.0.73/webapps'
			}}
		stage('Docker build'){
		    steps {
			sh 'docker build -t pushpak20497/pipelineimage11.1.2 .'
			}}
		stage('Docker Login'){
		    steps {
		withCredentials([string(credentialsId: 'pushpak20497', variable: 'Docker-hub')]) {
			sh 'docker login -u pushpak20497 -p${Docker-hub}'
}
			}
			
			}}
		stage('Push Image to Docker Hub') {         
    		    steps{                            
 			sh 'docker push pushpak20497/pipelineimage11.1.2:$BUILD_NUMBER'           
			echo 'Push Image Completed'       
    			}}
		
}}
