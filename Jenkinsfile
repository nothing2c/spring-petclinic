pipeline {
   agent any
environment {
        registryCredential = 'dock'
        dockerImage = ' '}
   
   tools {
        maven "maven"
    }

   stages {
        stage("Build") {
            steps {
                bat "mvn -version"
            }
        }
        stage("Junit Testing") {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
                }
            }
        }
        stage('Sonarqube Analysis') {
            // Reference https://github.com/jatinngupta/Jenkins-SonarQube-Pipeline/blob/master/Jenkinsfile
            steps {
                script {
                    //scannerHome = tool 'sonar-scanner';
                    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withCredentials([string(credentialsId: 'SonarToken', variable: 'SonarToken')]) {
                        bat "${scannerHome}/bin/sonar-scanner -X -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=${sonarLogin} -Dsonar.projectName=${env.JOB_NAME} -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=${env.JOB_BASE_NAME} -Dsonar.sources=src/main/java -Dsonar.java.libraries=target/* -Dsonar.java.binaries=target/classes -Dsonar.language=java"
                    }
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
