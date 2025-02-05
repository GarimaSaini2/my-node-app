pipeline {
    agent any

    environment {
        VM_IP = "20.55.27.218" // Replace with your VM's public IP
        VM_USER = "azureuser"       // Replace with your VM's username
        APP_DIR = "/home/azureuser/my-node-app/new-my-node-app" // Path on the VM where your app resides
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
                    sh 'npm install' // Assumes Linux VM
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
                    ssh -o StrictHostKeyChecking=no ${VM_USER}@${VM_IP} '
                        cd ${APP_DIR} &&
                        git pull origin main &&
                        npm install &&
                        pm2 restart app || pm2 start app.js --name my-node-app
                    '
                    """
                }
            }
        }
    }
}
