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
    image: maven:3.8.4-jdk-11
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
    stage ('Send to CD'){
      steps{
        cloudBeesFlowRunPipeline addParam: '{"pipeline":{"pipelineName":"git-pipline","parameters":"[{\\"parameterName\\": \\"Branch\\", \\"parameterValue\\": \\"${JOB_NAME}\\" }]"}}', configuration: 'CD-Config', pipelineName: 'git-pipline', projectName: 'SKO-UseCases'
      }}
  }
}
