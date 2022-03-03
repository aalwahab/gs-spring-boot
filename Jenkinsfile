pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '0'))
  }
  agent {
    kubernetes {
      //cloud 'kubernetes'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.3.9-jdk-11-alpine
    command: ['cat']
    tty: true
"""
    }
  }
  stages {
    stage('Run Maven') {
      steps {
        container('maven') {
          sh 'mvn deploy --settings ./complete/settings.xml -f ./complete/pom.xml'
        }
      }
    }
    stage ('Run Sonarqube'){
      steps{
        container('maven'){
          sh 'mvn sonar:sonar --settings ./complete/settings.xml -f ./complete/pom.xml'
        }}}
  }
}
