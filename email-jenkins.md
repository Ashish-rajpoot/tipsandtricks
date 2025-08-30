# Email Jenkins Pipeline
---
```bash
pipeline {
    agent any
    environment {
        EMAIL = 'ashishkumarrajput142@gmail.com'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // your build steps
            }
        }
    }
    post {
        success {
            mail to: "${EMAIL}",
                 subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) succeeded",
                 body: "Good news! The build succeeded."
        }
        failure {
            mail to: "${EMAIL}",
                 subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) failed",
                 body: "Unfortunately, the build failed. Please check the logs."
        }
    }
}

```
