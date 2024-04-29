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
					slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'devops', color: 'good', failOnError: true, message: 'welcome ', teamDomain: 'pipeline', tokenCredentialId: '86e861a5-0ca1-444a-b6a0-0bbad02af687'
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
