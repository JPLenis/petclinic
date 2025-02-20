pipeline {
    agent any

    // This triggers the pipeline every 3 minutes on Thursdays
    triggers {
        cron('H/3 * * * 4') 
    }

    environment {
        MAVEN_HOME = "${tool 'Maven 3.8.8'}" // Ensure Jenkins has Maven installed
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package' // Builds the project
            }
        }

        stage('Test and Generate Coverage') {
            steps {
                sh './mvnw test jacoco:report' // Runs tests and generates JaCoCo report
            }
        }

        stage('Publish Report') {
            steps {
                junit '**/target/surefire-reports/*.xml' // Publish test results
                jacoco execPattern: '**/target/jacoco.exec' // Publish JaCoCo coverage report
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
