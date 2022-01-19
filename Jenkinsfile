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
        sh "docker build -t lorconksu/devopscerttrainingproject3:${env.BUILD_ID} ."
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        sh 'echo $JOB_NAME'
      }
    }
    stage('Push') {
      steps {
        sh "docker push lorconksu/devopscerttrainingproject3:${env.BUILD_ID}"
      }
    }
    stage('Deploy to OCP') {
      steps {
        sh "oc set image deployment.apps/devops-cert-training-project3 devopscerttrainingproject3=lorconksu/devopscerttrainingproject3:${env.BUILD_ID} --kubeconfig /var/lib/jenkins/kubeconfig"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
