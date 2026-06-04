
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
                    sh '''
                    scp -o UserKnownHostsFile=/var/lib/jenkins/.ssh/known_hosts \
                        target/BaseWebApp-T10-1.0-SNAPSHOT.war \
                        ubuntu@172.31.39.251:/tmp/

                    ssh ubuntu@172.31.39.251 "
                        sudo rm -rf /var/lib/tomcat10/webapps/BaseWebApp-T10-1.0-SNAPSHOT*
                        sudo mv /tmp/BaseWebApp-T10-1.0-SNAPSHOT.war /var/lib/tomcat10/webapps/
                        sudo chown tomcat:tomcat /var/lib/tomcat10/webapps/BaseWebApp-T10-1.0-SNAPSHOT.war
                    "
                    '''
                }



            }
        }
    }
}

