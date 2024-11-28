pipeline {
    agent any

    tools {
         jdk 'Java17'
        maven 'Maven3' // The name configured in Global Tool Configuration
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
        
        ('SonarQube Code Analysis') {
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
    }
}