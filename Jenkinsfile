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
                }
            }
        }
    }
}