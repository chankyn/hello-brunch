#!/usr/bin/env groovy
pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }
    environment {
        TAG = "10.250.9.2:5050/root/hello-brunch:BUILD-1.${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Publish') {
            steps {
                withDockerRegistry(credentialsId: 'gitlab-registry', url: 'http://10.250.9.2:5050/root/hello-brunch') {
                    sh "docker tag hello-brunch:latest ${TAG}"
                    sh "docker push ${TAG}"
                }
            }
        }
    }
}
