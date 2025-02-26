pipeline {
    agent any

    environment {
        SERVER_IP = '44.203.38.242'
        DEPLOY_PATH = '/var/www/html'
        GIT_REPO = 'https://github.com/LauraOkafor/static-website-example.git'
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