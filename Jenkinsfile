pipeline {
    agent any  // Runs on any available Jenkins agent

    environment {
        REMOTE_HOST = "44.203.38.242"  // Replace with your EC2 Public IP
        REMOTE_USER = "ubuntu"  // Change if using a different SSH user
        PRIVATE_KEY = credentials('EC2_SSH_KEY')  // SSH key stored in Jenkins Credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/LauraOkafor/CICD-react-frontend.git' // Change this to your repo
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'  // Remove if not needed
                sh 'npm run build'  // Change based on your project’s build command
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh """
                        scp -o StrictHostKeyChecking=no -i $PRIVATE_KEY -r ./dist/* ${REMOTE_USER}@${REMOTE_HOST}:/home/ubuntu/static-site
                    """
                }
            }
        }

        stage('Restart Nginx') {
            steps {
                script {
                    sh """
                        ssh -o StrictHostKeyChecking=no -i $PRIVATE_KEY ${REMOTE_USER}@${REMOTE_HOST} <<EOF
                        sudo cp -r /home/ubuntu/static-site/* /var/www/html
                        sudo systemctl restart nginx
                        EOF
                    """
                }
            }
        }
    }
}