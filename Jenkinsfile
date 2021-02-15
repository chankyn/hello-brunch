#!/usr/bin/env groovy
pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }
    environment {
        TAG = "BUILD-1.${env.BUILD_NUMBER}"
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
                    sh "docker tag hello-brunch:latest 10.250.9.2:5050/root/hello-brunch:${TAG}"
                    sh "docker push 10.250.9.2:5050/root/hello-brunch:${TAG}"
                    sh "docker tag hello-brunch:latest 10.250.9.2:5050/root/hello-brunch:latest"
                    sh "docker push 10.250.9.2:5050/root/hello-brunch:latest"
                    sshagent(['gitlab-ssh']) {
                        sh "git tag ${TAG}"
                        sh "git push --tags"
                    }
                }
            }
        }
        stage("Deploy") {
            steps {
                withDockerRegistry(credentialsId: 'gitlab-registry-read', url: 'http://10.250.9.2:5050/root/hello-brunch') {
                    sshagent(['deploy-ssh']) {
                        sh "ssh -t -o 'StrictHostKeyChecking no' deploy@10.250.9.2 'docker-compose pull && docker-compose up -d'"
                    }
                }
            }
        }
    }
}
