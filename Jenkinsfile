pipeline {
    agent any

    environment {
        // Define any environment variables here, like AWS keys, etc.
        GITHUB_REPO = 'https://github.com/RamyaRaveesh/my-ci-cd-repo.git'
        BRANCH_NAME = 'main'
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
                    bat './venv/Scripts/pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Use batch commands for Windows
                    bat './venv/Scripts/pytest test_app.py'  // Adjust according to your test setup
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    // SSH and deploy to EC2 (replace with your deployment script or commands)
                    bat '''
                    ssh -o StrictHostKeyChecking=no -i C:\\path\\to\\your\\aws-key.pem ec2-user@${EC2_IP} << 'EOF'
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
        always {
            // Send email with build status
            emailext(
                subject: "Jenkins Build Status: ${currentBuild.currentResult}",
                body: """
                    <h3>Build Status: ${currentBuild.currentResult}</h3>
                    <p>Job: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</_
