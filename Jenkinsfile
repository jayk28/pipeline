pipeline {
	agent any 
	
	stages {
	    stage('Checkout') {
	        steps {
			checkout scm			       
		      }}
		stage('Build') {
	           steps {
			  sh '/home/devops/devops-tools/apache-maven-3.9.6/bin/mvn install'
	                 }}
		stage('Deployment') {
            steps {
                script {
                    if (env.ENV == 'QA') {
                        sh 'cp target/pipeline.war /home/devops/devops-tools/apache-tomcat-9.0.88/webapps'
                        echo "Deployment has been COMPLETED on QA!"
                    } else if (env.ENV == 'DEV') {
                        sh 'cp target/slack.war /home/devops/devops-tools/apache-tomcat-9.0.88/webapps'
                        echo "Deployment has been done on UAT!"
                    }
                }
            }
        }
		stage('Notify') {
			steps {
				script {
					slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'april', color: 'good', failOnError: true, message: 'hi', teamDomain: 'devops', tokenCredentialId: 'ae7b5a96-36d0-4613-b5d6-b6147bfd3668'
				}
			}
		}
	}
        post {
		always {
			emailext attachLog: true, body: '', compressLog: true, recipientProviders: [buildUser()], subject: 'pipeline', to: 'jaykamble370@gmail.com'
		}
	}
}
