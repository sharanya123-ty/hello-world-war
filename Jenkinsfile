pipeline {
    agent any
    environment {
        ARTIFACT_URL = 'http://13.201.185.43:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.1/hello-world-war-1.0.1.war'
        TOMCAT_PATH = '/opt/apache-tomcat-10.1.34'
    }
    stages {
        stage('Download Artifact') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'artifactory-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                   
                    cd /opt/apache-tomcat-10.1.34/webapps

                    # Correct usage of environment variables for curl
                    curl -L -u "\$USERNAME:\$PASSWORD" -O "\$ARTIFACT_URL"

                    cd ..
                    pwd
                    cd bin/
                    ./shutdown.sh
                    sleep 10
                    ./startup.sh
                    """
                }
            }
        }
    }
}
