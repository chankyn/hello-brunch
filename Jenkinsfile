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

        stage('Publish') {
            withDockerRegistry(credentialsId: 'gitlab-registry', url: 'http://10.250.9.2:5050/root/hello-brunch') {
                sh 'docker tag hello-brunch:latest 10.250.9.2:5050/root/hello-brunch:latest'
                sh 'docker push 10.250.9.2:5050/root/hello-brunch'
            }
        }
    }
    
}
