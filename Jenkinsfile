pipeline {
    agent any

    environment {
        AZURE_VM_USER = 'azureuser'  // Replace with your Azure VM username
        AZURE_VM_IP = '20.55.27.218' // Replace with your Azure VM IP
        AZURE_SSH_PRIVATE_KEY = credentials('azure-ssh-private-key') // Add your SSH private key in Jenkins credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }

        stage('Deploy to VM') {
            steps {
                script {
                    sh """
                    echo 'Deploying to VM...'
                    ssh -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} '
                        cd /home/azureuser/my-node-app &&
                        git pull origin main &&
                        npm install &&
                        pm2 restart app || pm2 start server.js --name my-node-app
                    '
                    """
                }
            }
        }
    }
}

  

