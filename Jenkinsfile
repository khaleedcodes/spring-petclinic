pipeline {
    agent any

environment {
    JAVA_HOME = "${tool 'JDK17'}"
    PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
}


    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/khaleedcodes/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                bat './mvnw clean package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Test & Coverage') {
            steps {
                bat './mvnw test'
            }
            post {
                always {
                    jacoco(execPattern: '**/target/jacoco.exec')
                }
            }
        }
    }

    triggers {
        cron('H/3 * * 4 *')
    }
}
