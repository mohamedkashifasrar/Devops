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

        stage('Sonarqube Analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                
             withSonarQubeEnv('SonarQube') {   
                withCredentials([string(credentialsId: 'Devops-token', variable: 'SONAR_TOKEN')]) {
                    sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.sources=frontend\
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                            """
                        }
                    }
                } 
            }
        }
    }
}
