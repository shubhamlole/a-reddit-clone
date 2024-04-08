pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-app"
        RELEASE = "1.0.0"
        DOCKER_USER = "shubham1166"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Clean Workspace"){
            stpes{
                cleanWs()
            }
        }
        stage("Git Checkout"){
            steps{
                git branch:'main', url:'https://github.com/shubhamlole/a-reddit-clone.git'
            }
        }
        stage("SonarQube Analysis"){
            steps{
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
                -Dsonar.projectKey=Reddit-Clone-CI'''
            }
        }
        stage("Quality Gate"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
                }
            }
            stage("Install Dependencies"){
                steps{
                    sh "npm install"
                }
            }
            stage("Trivy FS Scan"){
                sh "trivy fs . > trivyfs.txt"
            }
        }
    }
}
