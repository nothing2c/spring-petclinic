node {
   def mvnHome
environment {
        registryCredential = 'dock'
        dockerImage = ' '}


   stage('Preparation') { // for display purposes
        bat "mvn -version"
        bat "mvn clean install"
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }

   stage('Build Docker Image'){
           bat 'docker build -t nothing2c/spring-petclinic:latest .'
   }
   stage('Results') {

      junit '*/target/surefire-reports/TEST-.xml'
      archiveArtifacts 'target/*.jar'

   }
}
