pipeline {
    agent { label 'slave01' }
    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf hello-world-war'
                sh 'git clone https://github.com/sharanya123-ty/hello-world-war.git'
            }
        } 
       stage('build') {
                       steps {
                sh 'cd hello-world-war'
                sh 'mvn clean package'
            }
        }
        stage('deploy') {
           steps {
             sh 'cp /opt/Jenkins/workspace/pipeline_job001/target/hello-world-war-1.0.0.war /opt/apache-tomcat-10.1.34/webapps/'
               }
          }
            }
post {
    success {
        mail to: "sharavin422@gmail.com",
             subject: "Jenkins Job Success",
             body: "The Jenkins job completed successfully."
    }
    failure {
        mail to: "sharavin422@gmail.com",
             subject: "Jenkins Job Failed",
             body: "The Jenkins job failed. Check the logs for details."
    }
}
}
    
