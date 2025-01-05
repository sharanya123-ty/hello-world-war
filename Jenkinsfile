pipeline {
    agent { label 'slave1' }
    //tools {
     //   maven 'Maven-3.4.0' // Specify your Maven version if using Maven
     //   jdk 'JDK11'         // Specify your JDK version
    //}
    environment {
        SONAR_TOKEN = credentials('sonarcloud-token') // Store token in Jenkins credentials
    }
    
       stages 
    {
        stage('checkout') {             
            steps {
                sh 'rm -rf hello-world-war'
                sh 'git clone https://github.com/sharanya123-ty/hello-world-war.git/'
            }
        }
         stage('build') { 
            steps {
                sh 'cd hello-world-war'
                sh 'mvn clean package'
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install' // Adjust for your build tool
            }
        }
        //add your own sonar account details  
        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=sharanya123-ty_hello-world-war \
                      -Dsonar.organization=sharanya123-ty \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
    }
