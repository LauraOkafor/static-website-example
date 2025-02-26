stage('Checkout Code') {
    steps {
        git branch: 'master', url: "${GIT_REPO}"
    }
}pipeline {
    agent any

    environment {
        SERVER_IP = '44.203.38.242'
        DEPLOY_PATH = '/var/www/html'
        GIT_REPO = 'git@github.com:LauraOkafor/static-website-example.git'  // Use SSH for secure authentication
        SSH_CRED_ID = 'EC2_SSH_KEY'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', credentialsId: SSH_CRED_ID, url: GIT_REPO
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent([SSH_CRED_ID]) {
                    sh """
                        rsync -avz --delete ./ ubuntu@${SERVER_IP}:${DEPLOY_PATH}
                        ssh ubuntu@${SERVER_IP} 'sudo systemctl restart nginx'
                    """
                }
            }
        }
    }

    triggers {
        pollSCM('H/5 * * * *')  // Check for new commits every 5 minutes
    }
}