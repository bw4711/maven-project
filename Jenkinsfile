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
                        bat "winscp -i c:/bwprogramme/jenkinstutorial/bw-maven-project/part2/tomcat-demo2.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i c:/bwprogramme/jenkinstutorial/bw-maven-project/part2/tomcat-demo2.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
