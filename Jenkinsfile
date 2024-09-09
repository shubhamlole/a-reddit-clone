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
                git branch: 'main', url: 'https://github.com/shubhamlole1166/a-reddit-clone.git' 
            }
        }    
    }
}