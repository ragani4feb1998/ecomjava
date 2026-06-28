pipeline {

    agent any

    tools {
        maven 'Maven'
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

        stage('Deploy') {
            steps {
                sh '''
                cp target/*.war ${TOMCAT_DIR}/${WAR_NAME}
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
