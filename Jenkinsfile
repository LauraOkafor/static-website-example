pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/LauraOkafor/static-website-example.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "No build needed for static website"'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['EC2_SSH_KEY']) {
                    sh '''
                    scp -r * ubuntu@your-ec2-ip:/var/www/html
                    ssh ubuntu@your-ec2-ip 'sudo systemctl restart nginx'
                    '''
                }
            }
        }
    }
}