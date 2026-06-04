
pipeline {
    agent any

    environment {
        TOMCAT_HOST = "172.31.39.251"
        TOMCAT_USER = "ubuntu"
        TOMCAT_WEBAPPS = "/var/lib/tomcat10/webapps"
        WAR_NAME = "BaseWebApp-T10-1.0-SNAPSHOT.war"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Vikas14u/Java_WebApp_Base_T10.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(credentials: ['tomcat-ssh']) {
				    withEnv(["HOME=/home/ubuntu"]) {
                    sh """
                    scp target/${WAR_NAME} ${TOMCAT_USER}@${TOMCAT_HOST}:/tmp/
                    ssh ${TOMCAT_USER}@${TOMCAT_HOST} '
                        sudo systemctl stop tomcat10
                        sudo rm -rf ${TOMCAT_WEBAPPS}/${WAR_NAME}
                        sudo mv /tmp/${WAR_NAME} ${TOMCAT_WEBAPPS}/
                        sudo systemctl start tomcat10
                    '
                    """
                }
				}
            }
        }
    }
}

