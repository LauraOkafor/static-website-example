pipeline {
    agent any

    environment {
        SERVER_IP = "44.203.38.242"  // Replace with your EC2 instance's public IP
        SSH_USER = "ubuntu"  // Change based on your EC2 instance's user
        KEY_PATH = "/var/lib/jenkins/.ssh/cicd.pem" // Update with your SSH key path
        REPO_URL = "https://github.com/LauraOkafor/static-website-example.git"
        DEPLOY_PATH = "/home/ubuntu/static-website"
        NGINX_PATH = "/var/www/html"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh """
                    echo "Running basic tests..."
                    if [ -f index.html ]; then
                        echo "✅ index.html found!"
                    else
                        echo "❌ index.html is missing!"
                        exit 1
                    fi
                    """
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    sh """
                    # Securely copy files to EC2
                    scp -o StrictHostKeyChecking=no -i $KEY_PATH -r * $SSH_USER@$SERVER_IP:$DEPLOY_PATH/

                    # SSH into EC2 and deploy
                    ssh -o StrictHostKeyChecking=no -i $KEY_PATH $SSH_USER@$SERVER_IP << 'EOF'
                    sudo apt update && sudo apt install -y nginx
                    sudo rm -rf $NGINX_PATH/*
                    sudo cp -r $DEPLOY_PATH/* $NGINX_PATH/
                    sudo systemctl restart nginx
                    EOF
                    """
                }
            }
        }
    }
}