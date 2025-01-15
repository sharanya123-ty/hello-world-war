pipeline {
    agent { label 'slave' }
    environment {
        ARTIFACT_URL = 'http://3.110.105.203:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.1/hello-world-war-1.0.1.war'
        TOMCAT_PATH = '/opt/apache-tomcat-10.1.34'
    }
    stages {
        stage('Download Artifact') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'artifactory-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        // Download artifact and restart Tomcat
                        sh """
                            cd \$TOMCAT_PATH/webapps

                            # Download the artifact
                            curl -L -u "\$USERNAME:\$PASSWORD" -O "\$ARTIFACT_URL"

                            # Restart Tomcat
                            cd .. && pwd
                            cd bin/
                            ./shutdown.sh || true
                            sleep 3  # You can replace this with a more robust check if needed
                            ./startup.sh
                        """
                    }
                }
            }
        }
    }
}
