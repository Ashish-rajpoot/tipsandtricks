Absolutely! Here's how to **use environment variables** in your Jenkins pipeline for **each method** of setting env vars in Jenkins.

---

## 1. Using **Global Environment Variables** (Configured in **Manage Jenkins → Configure System**)

Say you added a global environment variable named `EC2_PUBLIC_IP` with value `1.2.3.4`.

### Jenkinsfile usage:

```groovy
pipeline {
    agent any
    environment {
        EC2_IP = "${EC2_PUBLIC_IP}"  // Use the global env var here
    }
    stages {
        stage('Example') {
            steps {
                echo "EC2 IP is ${EC2_IP}"
                sh "ssh ubuntu@${EC2_IP} 'echo Hello from EC2'"
            }
        }
    }
}
```

---

## 2. Using **Credentials Plugin** (For secrets like passwords or AWS keys)

Assume you stored AWS credentials (username/password or secret text) in Jenkins credentials with ID `aws-credentials`.

### Jenkinsfile usage:

```groovy
pipeline {
    agent any
    environment {
        AWS_REGION = 'ap-south-1'
    }
    stages {
        stage('Use AWS Credentials') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                    echo "AWS Access Key ID: $AWS_ACCESS_KEY_ID"
                    echo "AWS Secret Access Key: $AWS_SECRET_ACCESS_KEY"
                    aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                    aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                    aws configure set region ${AWS_REGION}
                    '''
                }
            }
        }
    }
}
```

If you used **Secret Text** credential type with ID `aws-token`:

```groovy
withCredentials([string(credentialsId: 'aws-token', variable: 'AWS_TOKEN')]) {
    sh 'echo "Using token: $AWS_TOKEN"'
}
```

---

## 3. Using **Environment Variables Defined Directly Inside Jenkinsfile**

You can define any env var inside the `environment` block of your pipeline:

```groovy
pipeline {
    agent any
    environment {
        EC2_IP = '1.2.3.4'
        AWS_REGION = 'ap-south-1'
    }
    stages {
        stage('Example') {
            steps {
                echo "Connecting to EC2 at ${EC2_IP}"
            }
        }
    }
}
```

---

## Summary Example with all three combined:

```groovy
pipeline {
    agent any
    environment {
        // Global env vars can be referenced directly or re-assigned here
        EC2_IP = "${EC2_PUBLIC_IP}"
        AWS_REGION = 'ap-south-1'
    }
    stages {
        stage('Use Global and Job Vars') {
            steps {
                echo "EC2 IP: ${EC2_IP}"
                echo "AWS Region: ${AWS_REGION}"
            }
        }
        stage('Use AWS Credentials') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                    aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                    aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                    aws configure set region ${AWS_REGION}
                    '''
                }
            }
        }
    }
}
```

---

If you want, I can also help you create a Jenkinsfile specific to your ECR + EC2 deployment that uses these environment methods!
