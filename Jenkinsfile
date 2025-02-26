pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/LauraOkafor/static-website-example.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'node -v'   // Check Node.js version
                sh 'npm -v'    // Check npm version
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['EC2_SSH_KEY']) {
                    sh '''
                    scp -r ./dist ubuntu@your-ec2-ip:/var/www/html
                    ssh ubuntu@your-ec2-ip 'sudo systemctl restart nginx'
                    '''
                }
            }
        }
    }
}