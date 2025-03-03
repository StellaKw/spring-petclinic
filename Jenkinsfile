pipeline {
    agent any

    triggers {
        cron('*/3 * * * 1')  // Every 3 minutes on Mondays
    }

    tools {
        maven 'Maven' // Ensure this matches your configured Maven tool name in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }

        stage('Test with JaCoCo') {
            steps {
                script {
                    if (isUnix()) {
                        sh '/opt/homebrew/bin/mvn test jacoco:report'  // Adjust the path as needed
                    } else {
                        bat 'mvn test jacoco:report'
                    }
                }
            }
            post {
                always {
                    // Publish the JaCoCo report as an HTML report in Jenkins
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Coverage Report"
                    ])
                }
            }
        }

        stage('Code Coverage with JaCoCo') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn jacoco:report'
                    } else {
                        bat 'mvn jacoco:report'
                    }
                }
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
