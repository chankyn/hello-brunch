#!/usr/bin/env groovy
pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage("Deploy") {
            steps {
                sh 'docker-compose up -d'
            }
        }
        stage('Security') {
            steps {
                sh 'trivy image --format json --output trivy.json hello-brunch:latest'
            }
            post {
                always {
                    recordIssues enabledForFailure: true, tool: trivy(pattern: 'trivy.json')
                }
            }
        }
    }
    
}
