pipeline {
   agent { label 'slave1'}  //slave1 = label name
    stages {
                stage('checkout') {
                   sh 'git checkout https://github.com/sharanya123-ty/hello-world-war/'
                   
          steps {
                echo 'Hello World'
            }
        }
        stage('Build') {
           sh 'mvn clean package'
          steps {
                echo 'Hello World'
            }
       stage('Deployment')
         
           post {
        always {
            emailext body: 'Hello sainath kadaverugu', recipientProviders: [$class: 'DevelopersRecipientProvider'], subject: 'After build message'
        }
        }
    }
}
