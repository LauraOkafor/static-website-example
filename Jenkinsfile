pipeline {
    agent any

    environment {
        SSH_KEY = credentials('EC2_SSH_KEY')  // Your Jenkins SSH Key ID
        SERVER_IP = "44.202.145.120"     // Replace with your EC2 IP
        SERVER_USER = "ubuntu"               // Change to 'ubuntu' if using Ubuntu
        GIT_REPO = "https://github.com/LauraOkafor/static-website-example.git"
        DEPLOY_DIR = "/var/www/html"
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
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}