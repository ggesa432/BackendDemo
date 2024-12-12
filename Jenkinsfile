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
                script {
                    def jarFile = bat(script: 'for /r %i in (target\\*.jar) do @echo %i', returnStdout: true).trim()
                    echo "Found JAR file: ${jarFile}"
                    if (jarFile) {
                        bat "java -jar ${jarFile}"
                    } else {
                        error("No JAR file found in the target directory!")
                    }
                }
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}

