pipeline {
    agent any
    stages {
        stage('Clone github code') {
            steps {
                git branch: 'main', credentialsId: '22127251_github', url: 'https://github.com/22127251/CICD_test_1.git'
            }
        }
        stage('Build image and upload to dockerhub') {
            steps {
                withDockerRegistry(credentialsId: '22127251_dockerhub', url: 'https://index.docker.io/v1/') {
                    bat 'docker build -t 22127251/cicd_test_1 .'
                    bat 'docker push 22127251/cicd_test_1'
                }
            }
        }
        stage('Check and stop existing container') {
            steps {
                script {
                    def containerId = bat(script: '@docker ps -q -f publish=3000', returnStdout: true).trim()
                    echo "Container ID: ${containerId}"
                    if (containerId) {
                        bat "docker stop ${containerId}"
                    }
                }
            }
        }
        stage('Pull image from dockerhub and run in container') {
            steps {
                bat 'docker pull 22127251/cicd_test_1'
                bat 'docker run -d -p 3000:80 22127251/cicd_test_1'
            }
        }        
    }
}