pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.5.0'
          args '-v /root/.m2:/root/.m2'
        }
      }
      steps {
          sh 'mvn clean install -B'
      }
    }
    stage('Build container') {
      agent any
      steps {
        script {
            pom = readMavenPom file: 'pom.xml'
            TAG = pom.version
            sh "docker build -t petclinic:${TAG} ."
        }
      }
    }
  }
}
