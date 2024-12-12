pipeline {
    agent any
    tools {
        maven 'maven 3.9.9'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat './mvnw.cmd clean package'
            }
        }
        stage('Test') {
            steps {
                bat './mvnw.cmd test'
            }
        }
        stage('Package') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
            steps {
                bat 'java -jar target/backend-0.0.1-SNAPSHOT.jar'
            }
        }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}

