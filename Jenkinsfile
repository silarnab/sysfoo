pipeline {

  agent any

  tools{
    maven 'Maven 3.6.1'
  }

  stages{
    
    stage('build'){
      steps{
	mvn compile
      }
    }

    stage('test'){
      steps{
	mvn clean test
      }
    }

    stage('package'){
      steps{
	mvn package -DskipTests
      }
    }
  }
}
