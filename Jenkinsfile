pipeline {
   agent any
   
   environment {
      registryCredential = 'dock'
      dockerImage = ' '
   }
   
   tools {
      maven "maven"
   }

   stages {
      stage("Build") {
         steps {
            bat "mvn -version"
            //bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            bat 'mvn install -DskipTests' 
         }
      }
      
      //stage("Junit Testing") {
         //steps {
            //bat 'mvn test'
         //}
         //post {
            //always {
               //junit allowEmptyResults: true, testResults: 'target/surefire-reports/**/*.xml' 
            //}
         //}
      //}
      
      stage('Sonarqube Analysis') {
         environment {
            scannerHome = tool 'SonarCub Scanner'
         }
         
         steps {
            withSonarQubeEnv('SonarCub') {
               bat "${scannerHome}/bin/sonar-scanner -X Dsonar.projectKey=${env.JOB_BASE_NAME}"
            }
         }
      }

      stage("Deploy") {
         steps {
            bat "mvn clean package"
         }
         post {
            success {
               archiveArtifacts 'target/*.jar'
            }
         }
      }
   }
}
