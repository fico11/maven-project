pipeline {
    agent any

     tools {
        maven 'localMAVEN'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost:9080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'http://localhost:9090', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

	stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
			sh "cp **/target/*.war /var/lib/tomcat9/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        build job: 'deploy-to-prod-to-potamadre'
                    }
                }
            }
        }
    }
}
