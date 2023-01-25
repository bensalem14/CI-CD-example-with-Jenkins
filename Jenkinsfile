pipeline {
  agent any
  stages {
    stage('test'){
      steps {
        bat 'gradlew test'
        archiveArtifacts 'build/test-results/'
        cucumber reportTitle: 'Raport',
        fileIncludePattern: 'target/report.json',
        trendsLimit: 10
        junit'build/test-results/test/TEST-Matrix.xml'
       
      }
    }
     
       stage('Code Analysis'){
      steps {
        withSonarQubeEnv('sonar'){
        bat 'gradlew sonarqube'
        }
      }
       }
     
      stage("Code Quality ") {
      steps {
          waitForQualityGate abortPipeline: true
      }
    }
     
     
     
       stage('build') {
      steps {
          bat "gradlew build"
          bat "gradlew javadoc"
          archiveArtifacts 'build/libs/*.jar'
          archiveArtifacts 'build/docs/'
      }
    }
     
     stage("Deploy ") {
      steps {
          bat "gradlew publish"
      }
    }
   
   
    stage("Notification") {
      steps {
          notifyEvents message: 'Good morrning   <b>Abderrahamne</b>', token: '8rFrh3GUQoCiwTENEgytE6Ed5BfH0BDc'
      }
    }

   
}
  post {
        failure {
            mail bcc: '', body: '''Error !!''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'jm_bensalem@esi.dz'
        }
  }

}
