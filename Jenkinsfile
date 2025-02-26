pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/LauraOkafor/static-website-example.git'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def indexExists = fileExists 'index.html'
                    if (indexExists) {
                        echo "✅ index.html found!"
                    } else {
                        error "❌ index.html is missing!"
                    }
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                sshagent(['EC2_SSH_KEY']) {
                    sh '''
                    echo "🚀 Deploying website to EC2..."
                    scp -r * ubuntu@your-ec2-ip:/home/ubuntu/static-website
                    '''
                }
            }
        }

        stage('Restart Nginx on EC2') {
            steps {
                sshagent(['EC2_SSH_KEY']) {
                    sh '''
                    ssh ubuntu@your-ec2-ip << EOF
                        echo "🔄 Restarting Nginx..."
                        sudo apt update && sudo apt install -y nginx
                        sudo rm -rf /var/www/html/*
                        sudo cp -r /home/ubuntu/static-website/* /var/www/html/
                        sudo systemctl restart nginx
                    EOF
                    '''
                }
            }
        }
    }
}