pipeline {
    agent any

    tools {
         jdk 'Java17'
        maven 'Maven3' // The name configured in Global Tool Configuration
    }
     environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://18.60.225.242:8081"
        NEXUS_REPOSITORY = "maven-releases"
        NEXUS_CREDENTIAL_ID = "NEXUS"
     }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
       stage('SonarQube Code Analysis') {
            steps {
                dir("${WORKSPACE}"){
                // Run SonarQube analysis for Python
                script {
                    def scannerHome = tool name: 'SONAR', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('MYSONAR') {
                         sh 'mvn clean package sonar:sonar'
                    }
                }
            }
         }
       } 
     stage('Upload Artifact'){
    steps{
       nexusArtifactUploader(
        nexusVersion: 'nexus3',
        protocol: 'http',
        nexusUrl: 'http://18.61.126.143:8081/',
        groupId: 'Devopa',
        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
        repository: 'maven-releases',
        credentialsId: 'NEXUS',
        artifacts: [
            [artifactId: projectName,
             classifier: '',
             file: 'target/spring-petclinic-3.4.0-SNAPSHOT.jar',
             type: 'jar']
        ]
     )
    }
     } 
 }

}
