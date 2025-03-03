pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1')  // This triggers the job every 3 minutes on Mondays
    }

    tools {
        maven 'Maven 3' // Make sure you have Maven tool configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Code Coverage with JaCoCo') {
            steps {
                sh 'mvn jacoco:report'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/site/jacoco/**', fingerprint: true
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
