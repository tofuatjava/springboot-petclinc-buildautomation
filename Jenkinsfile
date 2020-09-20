pipeline {
  agent {
    label "maven&&container&&linux"
  }
  
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
  }
  
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
        dir(path: 'checkout') {
          withMaven(maven: 'maven:latest', mavenSettingsConfig: '133c5646-7793-4964-9278-c9aa49b048ce') {
            sh 'mvn package'
          }
        }
      }
    }
    
    stage('Dependency Check') {
      steps {
        dependencyCheck additionalArguments: '--scan checkout', odcInstallation: 'dependency-check:latest'
        dir(path: 'checkout') {
          withMaven(maven: 'maven:latest', mavenSettingsConfig: '133c5646-7793-4964-9278-c9aa49b048ce') {
            sh 'mvn org.cyclonedx:cyclonedx-maven-plugin:makeAggregateBom'
          }
        } 
      }
      post {
        success {
          dependencyCheckPublisher pattern: ''
          dir(path: 'checkout') {
            dependencyTrackPublisher artifact: 'target/bom.xml', synchronous: false
          }
        }
      }
    }
    
    stage('Analysing Quality') {
      steps {
        dir(path: 'checkout') {
          withSonarQubeEnv('sonar-sonarqube') {
            withMaven(maven: 'maven:latest', mavenSettingsConfig: '133c5646-7793-4964-9278-c9aa49b048ce') {
              sh 'mvn sonar:sonar -Dsonar.dependencyCheck.xmlReportPath=dependency-check-report.xml'
            }
          }
        }
      }
    }

  }
}
