pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        git(url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'master')
      }
    }

    stage('build') {
      steps {
        sh './checkout/mvnw package'
      }
    }

  }
}