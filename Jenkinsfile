pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        dir(path: 'checkout') {
          git(url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'main')
        }
      }
    }

    stage('build') {
      steps {
        sh 'java --version'
        dir(path: 'checkout') {
          withMaven(maven: 'maven:latest', mavenSettingsConfig: '133c5646-7793-4964-9278-c9aa49b048ce') {
            sh 'mvn --version'
            sh 'mvn package'
          }
        }
      }
    }

  }
}
