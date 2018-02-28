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
			echo "vamos a desplegar, colo?"
			sh "cp **/target/*.war /mnt/1TB/apps/apache-tomcat-9.0.5_staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war /mnt/1TB/apps/apache-tomcat-9.0.5_prod/webapps"
                    }
                }
            }
        }
    }
}
