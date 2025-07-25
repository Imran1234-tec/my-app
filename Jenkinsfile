pipeline {
    agent any

    environment {
        S3_BUCKET = 'my-jenkins-app-artifact'
        AWS_REGION = 'us-east-2'
    }

    stages {
        stage('Clone Repository') {
            steps {
                sh |
                  git clone 'git@github.com:Imran1234-tec/my-app.git'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x build.sh && ./build.sh'
                sh 'ls -l app.zip || { echo "app.zip not found!"; exit 1; }'
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                    sh 'aws s3 cp app.zip s3://${S3_BUCKET}/app-${BUILD_NUMBER}.zip'
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
