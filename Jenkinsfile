node {
   def mvnHome
environment {
        registryCredential = 'dock'
        dockerImage = ' '}


   stage('Preparation') { // for display purposes
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
   
   stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build "nothing2c/spring-petclinic:latest"
                }
            }
        }

   //stage('Build Docker Image'){
           //bat 'docker build -t nothing2c/spring-petclinic:latest .'
   //}
   stage('Results') {

      junit '*/target/surefire-reports/TEST-.xml'
      archiveArtifacts 'target/*.jar'

   }
}
