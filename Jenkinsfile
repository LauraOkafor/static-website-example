pipeline {
    agent any

    environment {
        SERVER_IP = 'your-ec2-ip'
        DEPLOY_PATH = '/var/www/html'
        GIT_REPO = 'git@github.com:your-username/your-repo.git'
        SSH_CRED_ID = 'your-jenkins-ssh-credential-id'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', credentialsId: 'your-jenkins-git-credential-id', url: "${GIT_REPO}"
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-jenkins-ssh-credential-id']) {
                    sh """
                        rsync -avz --delete ./ ${SERVER_IP}:${DEPLOY_PATH}
                        ssh ${SERVER_IP} 'sudo systemctl restart nginx'
                    """
                }
            }
        }
    }

    triggers {
        pollSCM('* * * * *')  // Triggers on every push
    }
}