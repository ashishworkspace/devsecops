pipeline {
  agent any
  environment {
    dockerpass = credentials('docker-cred')
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
      stage('Docker Build and push') {
        steps {
          echo 'printenv'
          sh 'docker build -t ashishizofficial/spring-boot:""$GIT_COMMIT"" .'
          sh 'echo $dockerpass_PSW | docker login -u $dockerpass_USR --password-stdin'
          sh 'docker push ashishizofficial/spring-boot:""$GIT_COMMIT""'
        }
      }
      stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh "sed -i 's#replace#ashishizofficial/spring-boot:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh 'kubectl apply -f k8s_deployment_service.yaml'
        }
      }
      }
  }
}
