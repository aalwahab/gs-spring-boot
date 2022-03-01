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
    image: danielrmartin/sko:1.2
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
          sh 'mvn sonar:sonar -Dsonar.host.url=http://a21da2c1409fd4f33af9c8296d8b6170-1922071520.us-east-2.elb.amazonaws.com:9000/ -f ./complete/pom.xml'
        }}}
  }
}
