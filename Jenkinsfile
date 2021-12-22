pipeline {
  agent { label 'linux_deb' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('lorconksu-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh "docker build -t lorconksu/devopscertificationtrainingproject3:${env.BUILD_ID} ."
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh "id"
        sh "docker push lorconksu/devopscertificationtrainingproject3:${env.BUILD_ID}"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}