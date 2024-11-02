pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   

      stage('Unit test') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }     

        stage('Docker build and push') {
          steps {
            withDockerRegistry([url: "", credentialsId: "docker-hub"]) {
            sh 'printenv'
            sh 'docker build -t ykishore/numeric-app:${GIT_COMMIT} .'
            sh 'docker push ykishore/numeric-app:${GIT_COMMIT}'
            }
          }
        }
    }
}