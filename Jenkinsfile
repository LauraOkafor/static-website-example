pipeline {
    agent any

    triggers {
        githubPush()  // This triggers Jenkins on every GitHub push
    }

    environment {
        SSH_KEY = credentials('EC2_SSH_KEY')  // Use your Jenkins SSH key ID
        SERVER_IP = "44.201.242.144"    // Replace with your EC2 instance IP
        SERVER_USER = "ubuntu"                // Change if needed
        DEPLOY_DIR = "/var/www/html"
        GIT_REPO = "https://github.com/LauraOkafor/static-website-example.git"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: env.GIT_REPO
            }
        }

        stage('Install Nginx on EC2') {
            steps {
                script {
                    sh """
                        ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} << EOF
                        sudo apt update -y
                        sudo apt install -y nginx
                        sudo systemctl enable nginx
                        sudo systemctl start nginx
                        EOF
                    """
                }
            }
        }

        stage('Deploy Website') {
            steps {
                script {
                    sh """
                        scp -i ${SSH_KEY} -o StrictHostKeyChecking=no -r * ${SERVER_USER}@${SERVER_IP}:${DEPLOY_DIR}
                        ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} "sudo systemctl restart nginx"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Your website is live."
        }
        failure {
            echo "❌ Deployment Failed. Check Jenkins logs."
        }
    }
}