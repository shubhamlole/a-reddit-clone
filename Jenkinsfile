pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "shubham1166"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"            }

    stages{
        stage('Cleanup Workspace'){
            steps{
                cleanWs()
            }
        }
        stage('git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/shubhamlole/a-reddit-clone.git' 
            }
        }    
        stage("SonarQube Analysis"){
            steps{
                withSonarQubeEnv('SonarQube-Server'){
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
                        -Dsonar.projectKey=Reddit-Clone-CI'''
                }
            }
        }
        stage("Quality Gate"){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
            }
        }
        stage("Install Dependencies"){
            steps{
                sh "npm install"
            }
        }
    }
}