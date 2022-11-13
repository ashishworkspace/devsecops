pipeline {
  agent any
  environment{
    docker-pass=credentials('docker-cred')
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
          sh 'echo $docker-pass | docker login -u ashishizofficial --password-stdin'
          sh 'docker push ashishizofficial/spring-boot:""$GIT_COMMIT""'
        }
      }
  }
}
