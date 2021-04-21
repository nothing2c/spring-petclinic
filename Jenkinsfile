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
            //withSonarQubeEnv('SonarCub') {
               //bat "${scannerHome}/bin/sonar-scanner -X Dsonar.projectKey=${env.JOB_BASE_NAME}"
            //}
            script {
                    //scannerHome = tool 'sonar-scanner';
                    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                        bat "${scannerHome}/bin/sonar-scanner -X -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=${sonarLogin} -Dsonar.projectName=${env.JOB_NAME} -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=${env.JOB_BASE_NAME} -Dsonar.sources=src/main/java -Dsonar.java.libraries=target/* -Dsonar.java.binaries=target/classes -Dsonar.language=java"
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
