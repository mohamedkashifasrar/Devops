pipeline {
    agent any

    environment {
        PATH = "/usr/bin:/usr/local/bin:/bin:${env.PATH}"
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
                sh '''
                    echo "===== Node.js verification ====="
                    whoami
                    echo "PATH=$PATH"
                    which node
                    which npm
                    node --version
                    npm --version

                    echo "===== Installing dependencies ====="
                    npm install
                '''
            }
        }
    }
}









         
