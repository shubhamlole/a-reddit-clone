pipeline {
  agent any
  tools{
    jdk 'jdk17'
    nodejs 'node16'
  }
  environment{
    SCANNER_HOME= tool 'sonar-scanner'
    
  }
  stages {
    stage ("clean workspace") {
      steps {
        cleanWs()
      }
 }
  stage("checkout from git") {
    steps {
      git branch: 'main', url: 'https://github.com/shubhamlole/a-reddit-clone.git'
        }
     }
  stage("sonarqube analyazer") {
    steps {
      withSonarQubeEnv('sonarQube-Server'){
            sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
             -Dsonar.projectKey=Reddit-Clone-CI '''
      }
                       }
                       }
    stage("Quality Gate"){
      steps {
        script {
          waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
        }
      }
    }
    stage ("Install Dependencies") {
      steps {
        sh "npm install"
       }
    }
  }
}


  
