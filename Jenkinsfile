pipeline {
    agent any
	
	tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.208.178.7', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.206.223.166', description: 'Production Server')
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
                        bat ' winscp.exe -i "D:/Docs/Projects/GitRepo/learning-terraform/single ec2 instace/secret/private_key.pem" **/target/*.war ec2-user@18.208.178.7:/var/lib/tomcat7/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat ' "C:/Program Files/Git/usr/bin/scp" -i "D:/Docs/Projects/GitRepo/learning-terraform/single ec2 instace/secret/private_key.pem" **/target/*.war ec2-user@18.206.223.166:/var/lib/tomcat7/webapps'
                    }
                }
            }
        }
    }
}