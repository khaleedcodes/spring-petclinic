pipeline {
    agent any

    environment {
        JAVA_HOME = "C:/Program Files/Eclipse Adoptium/jdk-21.0.6.7-hotspot"
        PATH = "${env.JAVA_HOME}/bin;C:/Program Files/Apache/maven/bin;${env.PATH}"
    }

    triggers {
        cron('H/3 * * * 4') // Runs every 3 minutes on Thursdays
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat '.\\mvnw clean package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Test & Coverage') {
            steps {
                bat '.\\mvnw test'
            }
            post {
                always {
                    jacoco(execPattern: '**/target/jacoco.exec')
                }
            }
        }
    }
}
