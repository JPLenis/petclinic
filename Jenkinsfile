pipeline {
    agent any

    tools {
        maven 'Maven 3'  // You need this tool configured in Jenkins
        jdk 'JDK 21'     // You need this tool configured in Jenkins
    }

    stages {
        stage('Build and Test with Jacoco') {
            steps {
                sh 'mvn clean verify jacoco:report'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/jacoco.exec'
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}
