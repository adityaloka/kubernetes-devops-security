pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package"
              archive 'target/*.jar'
            }
        }  

        stage('maven test') {
            steps {
              sh "mvn test"
              
            }
        }  
    }
}