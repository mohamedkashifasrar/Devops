pipeline {
    agent any
   
     environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'devops-flo'
        CLOUDFRONT_DIST_ID= 'EKOA6Y638AYIJV'
        AWS_CREDENTIALS= credentials('aws-id')
        }

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
                            -Dsonar.projectKey=Devops \
                            -Dsonar.sources=frontend \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                            """
                        }
                    }
                } 
            }
        }

        stage('Deploy S3 Bucket'){
            steps{
                echo 'updating S3 Bucket'
                sh ''' 
                aws s3 sync frontend/dist/ \
                s3://${S3_BUCKET}/ \
                --delete \
                --region ap-south-1
                '''
                echo 'Frontend Uploaded Successfully'
            }      
        }
        stage('Cloudfront Deployment'){
            steps{
                echo 'Deploying...'
                sh ''' 
                  aws cloudfront create-invalidation \
                  --distribution-id ${CLOUDFRONT_DIST_ID} \
                  --paths "/*"
                  '''
  
            }
        }
    }
}
