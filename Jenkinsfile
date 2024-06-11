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
       def nexusUrl= 'nexus.naveencloud.online:8081'
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

        stage('nexus artifact upload'){
            steps{
              script{
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${nexusUrl}",
                groupId: 'com.example',
                version: "${appVersion}",
                repository: "backend",
                credentialsId: 'nexus',
                artifacts: [
                    [artifactId: "backend",
                    classifier: '',
                    file: 'backend-' + "${appVersion}" + '.zip',
                    type: 'zip']
                ]
            )
              }
            }
        }
        stage("deploy"){
            stage{
                def params =[
                       string(name: 'appVersion',value="${appVersion}")
                ]
                script{
                     build job: 'backend-deploy', parameters: params,wait: false
                }
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