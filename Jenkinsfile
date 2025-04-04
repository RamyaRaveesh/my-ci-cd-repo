pipeline {
    agent any

    environment {
        // Define any environment variables here, like AWS keys, etc.
        GITHUB_REPO = 'https://github.com/yourusername/my-ci-cd-repo.git'
        BRANCH_NAME = 'main'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/my-ci-cd-repo.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                script {
                    sh 'python3 -m venv venv'
                    sh './venv/bin/pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh './venv/bin/pytest test_app.py'  // Adjust according to your test setup
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    // SSH and deploy to EC2 (replace with your deployment script or commands)
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i /path/to/your/aws-key.pem ec2-user@${EC2_IP} << 'EOF'
                        cd /path/to/project
                        git pull origin main
                        sudo systemctl restart your-app-service
                    EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
            // Send Slack notification (optional)
            slackSend (channel: '#ci-cd', message: 'Build and deployment successful!')
        }
        failure {
            echo 'Build or deployment failed!'
            // Send Slack notification (optional)
            slackSend (channel: '#ci-cd', message: 'Build or deployment failed!')
        }
    }
}
