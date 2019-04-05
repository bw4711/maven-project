pipeline {
    agent any
	tools {
        maven 'myMaven'
    }

	stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
        }
    }
}