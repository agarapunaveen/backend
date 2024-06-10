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
        Deploy_to = "production"
        greetings = "good mornint"
    }
    stages {
        stage('Init') {
            steps {
                sh """
                 npm install
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