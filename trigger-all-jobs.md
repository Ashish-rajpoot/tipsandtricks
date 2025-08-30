Here's a **complete Jenkins pipeline chain** setup with 3 separate Jenkinsfiles:

---

## ✅ Assumptions:

You have 3 Jenkins pipelines:

* `push-pipeline` → uses `Jenkinsfile-push`
* `pull-pipeline` → uses `Jenkinsfile-pull`
* `email-pipeline` → uses `Jenkinsfile-email`

All these Jenkinsfiles are in the **same Git repository** (or different if you prefer — just adjust the job configs accordingly).

---

## 📁 Your repo structure:

```
your-repo/
├── Jenkinsfile-push
├── Jenkinsfile-pull
└── Jenkinsfile-email
```

---

## 1️⃣ `Jenkinsfile-push`

```groovy
pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '156172784305'
        AWS_REGION = 'ap-south-1'
        ECR_REPO_NAME = 'acr-repo'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} \
                | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${ECR_REPO_NAME}:${IMAGE_TAG} .
                docker tag ${ECR_REPO_NAME}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Trigger Pull Pipeline') {
            steps {
                build job: 'pull-pipeline', parameters: [
                    string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")
                ]
            }
        }
    }
}
```

---

## 2️⃣ `Jenkinsfile-pull`

> Ensure this job is **parameterized** in Jenkins:

* Go to job → Configure → "This project is parameterized"
* Add `IMAGE_TAG` (String)

```groovy
pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest')
    }

    environment {
        AWS_ACCOUNT_ID = '156172784305'
        AWS_REGION = 'ap-south-1'
        ECR_REPO_NAME = 'acr-repo'
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} \
                | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Pull Docker Image') {
            steps {
                sh 'docker pull ${ECR_URI}'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop app || true
                docker rm app || true
                docker run -d --name app -p 80:80 ${ECR_URI}
                '''
            }
        }

        stage('Trigger Email Pipeline') {
            steps {
                build job: 'email-pipeline', parameters: [
                    string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")
                ]
            }
        }
    }
}
```

---

## 3️⃣ `Jenkinsfile-email`

> Also parameterized to accept `IMAGE_TAG`

```groovy
pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest')
    }

    environment {
        RECIPIENT = 'ashishrajput142@gmail.com'
    }

    stages {
        stage('Send Email Notification') {
            steps {
                mail to: "${RECIPIENT}",
                     subject: "Deployment of image: ${IMAGE_TAG} completed",
                     body: "The Docker image (${IMAGE_TAG}) has been pulled and deployed successfully."
            }
        }
    }
}
```

---

## ✅ Recap of Job Setup:

* **Job 1 (push-pipeline):** Uses `Jenkinsfile-push`
* **Job 2 (pull-pipeline):** Uses `Jenkinsfile-pull` → must be parameterized with `IMAGE_TAG`
* **Job 3 (email-pipeline):** Uses `Jenkinsfile-email` → also parameterized

---

Let me know if you want to trigger this from GitHub pushes or add Slack/email failure notifications too.
