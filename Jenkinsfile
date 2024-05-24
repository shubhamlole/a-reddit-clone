pipeline {
  agent any 
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
  }
}


  
