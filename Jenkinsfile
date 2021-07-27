pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts(artifacts: 'target/sysfoo.war', onlyIfSuccessful: true)
      }
    }

  }
  tools {
    maven 'Maven 3.6.1'
  }
}