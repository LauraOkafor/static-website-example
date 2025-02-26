pipeline {
    agent any

    environment {
        SERVER_IP = '44.203.38.242'
        DEPLOY_PATH = '/var/www/html'
        GIT_REPO = 'https://github.com/LauraOkafor/static-website-example.git'
        SSH_CRED_ID = 'EC2_SSH_KEY'  // Update with your actual SSH credential ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: '*/master']],
                        userRemoteConfigs: [[
                            url: "${GIT_REPO}",
                            credentialsId: 'your-jenkins-git-credential-id'  // Update with your actual Git credential ID
                        ]]
                    ])
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sshagent(credentials: [SSH_CRED_ID]) {
                        sh """
                            rsync -avz --delete . ${SERVER_IP}:${DEPLOY_PATH}
                            ssh ${SERVER_IP} 'sudo systemctl restart nginx'
                        """
                    }
                }
            }
        }
    }

    triggers {
        pollSCM('H/5 * * * *')  // Polls every 5 minutes for changes
    }
}