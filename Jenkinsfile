pipeline {
    agent any
	
	tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.90.109.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.90.75.34', description: 'Production Server')
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
					/* C:\Jenkins\workspace\package\webapp\target */
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat ' "C:/Program Files/Git/usr/bin/scp" -i "D:/Docs/Projects/GitRepo/learning-terraform/single ec2 instace/secret/private_key.pem" **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat ' "C:/Program Files/Git/usr/bin/scp" -i "D:/Docs/Projects/GitRepo/learning-terraform/single ec2 instace/secret/private_key.pem" **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps'
                    }
                }
            }
        }
    }
}