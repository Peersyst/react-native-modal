pipeline {
    agent any
    environment {
        HOME = '.'
    }
    stages {
        stage('Install') {
            agent {
                docker {
                    image 'node:16'
                    reuseNode true
                }
            }
            steps {
                withCredentials([string(credentialsId: 'npm-publish-token', variable: 'NPM_TOKEN')]) {
                    sh 'yarn --frozen-lockfile'
                }
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:16'
                    reuseNode true
                }
            }
            steps {
                withCredentials([string(credentialsId: 'npm-publish-token', variable: 'NPM_TOKEN')]) {
                    sh 'yarn build'
                }
            }
        }
        stage('Publish') {
            agent {
                docker {
                    image 'node:16'
                    reuseNode true
                }
            }
            when {
                branch 'master'
            }
            steps {
                withCredentials([string(credentialsId: 'npm-publish-token', variable: 'NPM_TOKEN')]) {
                    sh "npm publish"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}