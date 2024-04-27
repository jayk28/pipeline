pipeline {
	agent any 
	
	stages {
	    stage('Checkout') {
	        steps {
			checkout scm			       
		      }}
		stage('Build') {
	           steps {
			  sh '/home/devops/devops_tool/apache-maven-3.9.6/bin/mvn install'
	                 }}
		stage('Deployment'){
		   steps {
		sh 'cp target/pipeline.war /home/devops/devops_tool/apache-tomcat-9.0.88/webapps'
			}}	
}}
