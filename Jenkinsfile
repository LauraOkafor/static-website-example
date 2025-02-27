pipeline {
    agent any

    triggers {
        githubPush()  // Auto-trigger pipeline on push
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LauraOkafor/static-website-example.git'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/*', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['EC2_SSH_KEY']) {
                    sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 3.88.38.231 >> ~/.ssh/known_hosts
                        scp -r * ubuntu@3.88.38.231:/tmp/static-site
                        
                        ssh -o StrictHostKeyChecking=no ubuntu@3.88.38.231 "
                            sudo apt update && sudo apt install -y nginx
                            sudo systemctl start nginx
                            sudo rm -rf /var/www/html/*
                            sudo cp -r /tmp/static-site/* /var/www/html/
                            sudo systemctl restart nginx
                        "
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}