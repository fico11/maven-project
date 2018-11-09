pipeline {
	agent any

	tools {
		maven 'localMAVEN'
	}

	parameters {
		string(name: 'tomcat_dev', defaultValue: 'http://localhost:9080', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: 'http://localhost:9090', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages{
		stage('Build'){
			steps {
				bat 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage ('Deployments'){
			parallel{
				stage ('Deploy to Staging'){
					steps {						
						echo "vamos a desplegar en ${tomcat_dev}"
						bat "winscp **\\target\\*.war C:\\apps\\Tomcat\\apache-tomcat-9.0.5_staging\\webapps"
						//bat "C:\\\"Program Files\"\\Linux\\pscp -scp -i C:/Users/sidne/Development/ssh-key/tomcat-demo.pem **\\target\\*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
						//bat "C:\\\"Program Files\"\\PuTTY\\pscp -scp **\\target\\*.war C:\\apps\\Tomcat\\apache-tomcat-9.0.5_staging\\webapps
					}
				}

				stage ("Deploy to Production"){
					steps {
						echo "vamos a desplegar en ${tomcat_prod}"
						bat "winscp **/target/*.war C:\apps\Tomcat\apache-tomcat-9.0.5_prod\webapps"
					}
				}
			}
		}
	}
}
