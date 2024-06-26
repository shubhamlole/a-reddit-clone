pipeline{
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "rukminihub"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")

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
        stage("sonarqube analysis") {
            steps{
                withSonarQubeEnv('SonarQube-Server') {
                    sh '''${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
                    -Dsonar.projectKey=Reddit-Clone-CI'''
                }
            }
        }
        stage("Quality Gate"){
            steps{
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
                }
            }
        }
        stage('Install Dependencies') {
            steps{
              sh "npm install"
            }
        }
        stage('trivy fs scan') {
            steps{
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage('build and push docker image') {
            steps{
                script{
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')            
                    }
                }
            }
        }
        stage('Trivy Image Scan') {
            steps {
                script {
                    sh '''
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/root/.cache/ aquasec/trivy image \
                    --no-progress \
                    --scanners vuln \
                    --severity HIGH,CRITICAL \
                    --exit-code 0 \
                    --format table \
                    rukminihub/reddit-clone-pipeline:latest > trivyimage.txt
                 '''
                }
            }    
                
        }
        stage('Clean Up Artifact'){
            steps{
                script {

                 sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                 sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Trigger CD Job'){
            steps{
                 script {
                    sh "curl -v -k --user rukmini:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-43-205-128-90.ap-south-1.compute.amazonaws.com:8080/job/Reddit-Clone-CD/buildWithParameters?token=gitops-token'"
                }
            }
        }
          
    }
    post{
        always {
            emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME} </br>" + 
                  "BUild Number: ${env.BUILD_NUMBER} </br>" + 
                  "URL: ${env.BUILD_URL} </br>",
            to: 'loleshubham46@gmail.com',
            attachmentsPattern: 'trivyfs.txt ,trivyimage.txt'
        }
    }
}


