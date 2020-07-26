pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        dir(path: 'checkout') {
          git(url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'master')
        }

      }
    }

    stage('build') {
      steps {
        dir(path: 'checkout') {
          sh './mvnw package'
        }

      }
    }

  }
}