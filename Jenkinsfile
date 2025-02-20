pipeline {
    agent any

    triggers {
        cron('H/3 * * * 4') // Every 3 minutes on Thursdays
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64' // Ensure correct JDK version
        PATH = "/usr/lib/jvm/java-17-openjdk-amd64/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh './mvnw clean package'
                }
            }
        }

        stage('Test and Generate Coverage') {
            steps {
                script {
                    sh './mvnw test'
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // Publish test results
                }
            }
        }

        stage('JaCoCo Report') {
            steps {
                script {
                    sh './mvnw jacoco:report'
                }
            }
            post {
                success {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'JaCoCo Coverage Report'
                    ])
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
