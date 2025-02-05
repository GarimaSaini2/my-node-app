pipeline {
    agent any

    environment {
        VM_IP = credentials('AZURE_VM_IP')
        VM_USER = credentials('AZURE_VM_USER')
        APP_DIR = "/home/azureuser/my-node-app"
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

        stage('Deploy to Azure VM') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${VM_USER}@${VM_IP} '
                        cd ${APP_DIR} &&
                        git pull origin main &&
                        npm install &&
                        npm run build &&
                        pm2 restart app || pm2 start app.js --name my-node-app
                    '
                    """
                }
            }
        }
    }
}

