pipeline {
    agent any
       stages 
    {
        stage('checkout') {             
            steps {
                sh 'git clone https://github.com/sharanya123-ty/hello-world-war/'
            }
        }
         stage('build') { 
            steps {
                sh 'cd hello-world-war'
                sh 'mvn clean package'
            }
        }
    }
}
