pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'Frontend',
                    url: 'https://github.com/mohamedkashifasrar/Devops.git'
            }
        }

        stage('Node and NPM Test') {
            steps {
                sh '''
                    echo "===== JENKINS ENVIRONMENT ====="
                    whoami
                    echo "PATH=$PATH"

                    echo "===== NODE ====="
                    /usr/bin/node --version

                    echo "===== NPM ====="
                    /usr/bin/npm --version

                    echo "===== WHICH ====="
                    command -v node || true
                    command -v npm || true
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    echo "===== NPM INSTALL ====="
                    /usr/bin/npm install
                '''
            }
        }
    }
}
