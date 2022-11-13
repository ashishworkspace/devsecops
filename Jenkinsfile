pipeline {
  agent any
  environment{
    dockerhub=credentials('docker-cred')
  }

  stages {
      stage('Build Artifact') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archive 'target/*.jar'
      }
      }
      stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
      }
      stage('Docker Build and push'){
        steps{
          echo 'printenv'
          sh 'docker build -t ashishizofficial/spring-boot:""$GIT_COMMIT"" .'
          sh 'echo $dockerhub'
          // sh 'docker push ashishizofficial/spring-boot:""$GIT_COMMIT""'
        }
      }
  }
}
