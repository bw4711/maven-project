pipeline {
    agent any
	tools {
        maven 'myMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.15.128.204', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.15.80.99', description: 'Production Server')
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
                        sh "cp 'c:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war' c:/bwprogramme/apache-tomcat-8.5.39/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp 'c:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war' c:/bwprogramme/apache-tomcat-8.5.39-prod/webapps"
                    }
                }
            }
        }
    }
}