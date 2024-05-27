pipeline{
    agent any
    tools {
        jdk 'jdk11'
        nodejs 'node16'
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
    }
}