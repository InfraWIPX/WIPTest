pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'sudo docker build . -t 10-frontend'
        sh 'sudo docker tag 10-frontend registry.wip.camp/10-frontend:$BUILD_NUMBER'
        sh 'sudo docker tag 10-frontend registry.wip.camp/10-frontend'
      }
    }
    stage('push') {
      steps {
        sh 'sudo docker push registry.wip.camp/10-frontend:$BUILD_NUMBER'
        sh 'sudo docker push registry.wip.camp/10-frontend'
      }
    }
    stage('deploy') {
      steps {
        sh 'sudo kubectl rolling-update 10-frontend --image registry.wip.camp/10-frontend:$BUILD_NUMBER'
      }
    }
    stage('clean') {
      steps {
        sh 'sudo docker image rm registry.wip.camp/10-frontend:$BUILD_NUMBER'
        sh 'sudo docker image rm registry.wip.camp/10-frontend'
      }
    }
  }
  post {
    success {
      steps {
        sh 'echo success'
      }
    }
    failure {
      steps {
        sh 'echo failure'
      }
    }
  }
}