pipeline{
    agent any
    tools {
        jdk 'jdk11'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "rukminihub"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}" - "${BUILD_NUMBER}"

            }
    stages{
        stage("clean workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Git Checkout"){
            steps{
                git url:'https://github.com/shubhamlole/a-reddit-clone.git', branch: 'main'
            }
        }
        stage("sample echo"){
            steps{
            echo "build_url: ${env.BUILD_URL}"
            }
        }
    }
       
}