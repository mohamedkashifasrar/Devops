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
                    pwd

                    echo "===== NODE ====="
                    /usr/bin/node --version

                    echo "===== NPM ====="
                    /usr/bin/npm --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('frontend') {
                    sh '''
                        echo "===== FRONTEND DIRECTORY ====="
                        pwd

                        echo "===== FILES ====="
                        ls -la

                        echo "===== PACKAGE.JSON ====="
                        ls -l package.json

                        echo "===== NPM INSTALL ====="
                        /usr/bin/npm install
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                dir('frontend') {
                    sh ''' 
                        /usr/bin/npm run build
                    '''
                }
            }
        }
    }
}
