pipeline {
    agent {
        label 'node'
    }
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
   
    environment{
       def appVersion = ''
    }
    stages {
        stage('read the version'){
            steps{

              script { 
                def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh """
                 npm install
                 ls -ltr
                """
            }
        }
        stage('Build'){
            steps{
               sh """
                    zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                    ls -ltr
                """
            }
        }
    }
        post {
            always{
                echo ' i will always say hello again'
                deleteDir()
            }
            success{
                echo ' i will run the pipeline success'
            }
            failure{
                echo ' i will run the pipeline failure'
            }
        }
}