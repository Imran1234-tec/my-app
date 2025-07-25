pipeline {
    agent any

    environment {
        S3_BUCKET = 'my-jenkins-app-artifact'
        AWS_REGION = 'us-east-2'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone repository correctly
                git url: 'git@github.com:Imran1234-tec/my-app.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'chmod +x build.sh'
                    sh './build.sh'

                    // Check for the app.zip file existence
                    if (!fileExists('app.zip')) {
                        error('app.zip not found!')
                    }
                }
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                    sh "aws s3 cp app.zip s3://${S3_BUCKET}/app-${BUILD_NUMBER}.zip"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful'
        }
        failure {
            echo '❌ Deployment failed'
        }
    }
}
