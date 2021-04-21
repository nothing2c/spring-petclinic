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
            bat 'mvn -Dmaven.test.failure.ignore=true install' 
            }
         }
      }
      
      stage("Junit Testing") {
         steps {
            bat 'mvn test'
         }
         post {
            always {
               junit allowEmptyResults: true, testResults: 'target/surefire-reports/**/*.xml' 
            }
         }
      }
      
      stage('Sonarqube Analysis') {
         environment {
            scannerHome = tool 'SonarQubeScanner'
         }
         
         steps {
            withSonarQubeEnv('sonarqube') {
               bat "${scannerHome}/bin/sonar-scanner -X"
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
