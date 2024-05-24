pipeline {
  agent any
  tools{
    jdk 'jdk17'
    nodejs 'node16'
  }
  environment{
    SCANNER_HOME= tool 'sonar-scanner'
    APP_NAME = "reddit-clone-CI"
    RELEASE = "1.0.0"
    DOCKER_USER = "rukminihub"
    DOCKER_PASS = "dockerhub"
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    
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
    stage ("TRIVY FS SCAN") {
      steps {
        sh "trivy fs .> trivyfs.txt"
        }
     }
  stage ("build & push Docker Image") {
    steps {
      script {
        docker.withRegistry('',DOCKER_PASS) {
          docker_image = docker.build "${IMAGE_NAME}"
        }
        docker.withRegistry('',DOCKER_PASS) {
          docker_image.push("${IMAGE_TAG}")
          docker_image.push('latest')
           }
        }
      }
    }
  }
}
  
