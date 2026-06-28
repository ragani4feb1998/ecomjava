pipeline {

    agent any

    tools {
        maven 'Maven-3.8.7'
    }

    environment {
        WAR_NAME = "mycart.war"
        TOMCAT_DIR = "/var/lib/tomcat10/webapps"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-key',
                    url: 'git@github.com:ragani4feb1998/ecomjava.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                sh 'ls -lh target'
            }
        }
	
	stage('Copy WAR') {
    	steps {
        	sh '''
        	cp target/mycart.war /tmp/mycart.war
        	'''
    		}
	}


        stage('Deploy') {
            steps {
                sh '''
        		scp /tmp/mycart.war jenkinsdeploy@YOUR_EC2_PRIVATE_IP:/tmp/mycart.war

        		ssh jenkinsdeploy@172.31.7.216 "
            		sudo cp /tmp/mycart.war /var/lib/tomcat10/webapps/mycart.war
        		"
        		'''
            }
        }

    }

    post {

        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}
