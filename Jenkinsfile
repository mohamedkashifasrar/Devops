pipeline {
    agent any

    tools {
      nodejs 'Frontend'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'Frontend',
                    url: 'https://github.com/mohamedkashifasrar/Devops.git'
            }
        }

        stage('Install') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install'
            }
        }
    }
}










         
