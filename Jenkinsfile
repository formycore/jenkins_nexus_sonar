// https://github.com/vikash-kumar01/demo-counter-app.git
pipeline {
    agent any
    tools {
        maven '3.6.3'
    }
    stages {
        stage ('Git Checkout'){
            steps {
                git 'https://github.com/formycore/jenkins_nexus_sonar.git'
            }
        }
        stage ('Test'){
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration Test'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Static Code Analysis'){
            steps {
                script {
            withSonarQubeEnv(credentialsId: 'secreta') {
                sh 'mvn clean package sonar:sonar'
            }
            }
            }
        }
        stage ('Quality Gate Analysis'){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'secreta'
// here we will get error checking for the sonarqube task some <id>
// we need to create a sonarqube webhook
//sonarqube -> administration -> configuration-> webhooks -> Name: jenkins_webhook
// URL: http://<jenkins-url:8080>/sonarqube-webhook/ -> save it
// re run it 
                    
                }
            }
        }
        stage ('Upload artifact to nexus'){
            steps {
                script{
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                            ],
                            credentialsId: 'nexus-auth',
                            groupId: 'com.example', 
                            nexusUrl: '34.29.226.153:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'demoapp-release', 
                            version: '1.0.0'
                }
            }
        }
    }
}