pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh 'mvn clean verify'
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }

        stage('Unit Tests - JUnit and Jacoco') {
            steps {
                echo 'Publishing JUnit test results and JaCoCo coverage'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'target/surefire-reports/*.xml'

                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                withDockerRegistry(credentialsId: 'docker-hub-pass', url: '') {
                    sh 'docker build -t adityalokapalli309/numeric-app:"$GIT_COMMIT" .'
                    sh 'docker push adityalokapalli309/numeric-app:"$GIT_COMMIT"'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }

        stage('Kubernetes Deployment - DEV') {
           steps {
              withKubeConfig([credentialsId: 'kubeconfig']) {
                 sh "sed -i 's#replace#adityalokapalli309/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                 sh "kubectl apply -f k8s_deployment_service.yaml"
                }
            }
         }
    }
}
