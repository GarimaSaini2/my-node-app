pipeline {
    agent any
    environment {
        SSH_CREDENTIALS_ID = 'ssh-key'  // Add SSH Key in Jenkins Credentials
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install' // or pip install -r requirements.txt
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' // or pytest
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ssh-key']) {
                    sh '''
                    ssh azureuser@20.55.27.218 << 'EOF'
                    cd /var/www/app
                    git pull origin main
                    npm install
                    pm2 restart all
                    EOF
                    '''
                }
            }
        }
    }
}
