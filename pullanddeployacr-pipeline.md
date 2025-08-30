---
# Pull and Deploy Image to Docker

```bash
pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '156172784305'
        AWS_REGION = 'ap-south-1'
        ECR_REPO_NAME = 'acr-repo'
        IMAGE_TAG = 'latest'
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Pull Image from ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

                docker pull ${ECR_URI}
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop app || true && docker rm app || true
                docker run -d --name app -p 8000:80 ${ECR_URI}
                '''
            }
        }
    }
}



```
