pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package"
              archive 'target/*.jar'
            }
        }  

        stage('Unit test') {
            steps {
              sh "mvn test"
              
            }
        }  
    }
}
