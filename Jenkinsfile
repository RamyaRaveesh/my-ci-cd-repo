pipeline {
    agent any

    environment {
        // Define any environment variables here, like AWS keys, etc.
        GITHUB_REPO = 'https://github.com/RamyaRaveesh/my-ci-cd-repo.git'
        BRANCH_NAME = 'main'
        EC2_IP = '13.51.70.213'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/RamyaRaveesh/my-ci-cd-repo.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                script {
                    // Use batch commands for Windows
                    bat 'python -m venv venv'
                    bat 'venv\\Scripts\\pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Use batch commands for Windows
                    bat 'venv\\Scripts\\pytest test_app.py'  // Adjust according to your test setup
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
            // SSH and deploy to EC2 (replace with your deployment script or commands)
            bat '''
            ssh -o StrictHostKeyChecking=no -i C:\\Users\\ravee\\Downloads\\my-sample-app.pem ubuntu@${EC2_IP} "cd /path/to/project && git pull origin main && sudo systemctl restart my-python-app"
            '''
        }
            }
        }
    }

   post {
        always {
            // Send email with build status
            emailext(
                subject: "Jenkins Build Status: ${currentBuild.currentResult}",
                body: """
                    <h3>Build Status: ${currentBuild.currentResult}</h3>
                    <p>Job: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                    <p>Test Report: <a href="${env.BUILD_URL}testReport">${env.BUILD_URL}testReport</a></p>
                """,
                to: "ramyashridharmoger@gmail.com"  // Replace with the email recipient's address
            )
        }
    }
}
