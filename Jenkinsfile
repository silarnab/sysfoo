pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn clean test'
      }
    }

    stage('package') {
      when { branch 'master' }
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts(artifacts: 'target/sysfoo.war', onlyIfSuccessful: true)
      }
    }

    stage('Docker BnP') {
      when { branch 'master' }
      agent any
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'docker_login') {
            def dockerImage = docker.build("silarnab/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }
    
    stage('Deploy to Dev') {
      when {
        beforeAgent true
        branch 'master'
      }
      agent any
      steps {
        echo 'Deploying to Dev Compose'
        sh 'docker-compose up -d'
      }
    }
  }
  tools {
    maven 'Maven 3.6.1'
  }
}
