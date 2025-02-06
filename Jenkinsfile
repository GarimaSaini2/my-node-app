pipeline {
    agent any

    environment {
        AZURE_VM_USER = 'azureuser'  // Azure VM username
        AZURE_VM_IP = '20.55.27.218' // Azure VM IP
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
                    sshagent(['azure-ssh-private-key']) {  // Reference the SSH credential in Jenkins
                        sh """
                        echo 'Deploying to Azure VM...'
                        ssh -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} << EOF
                            cd /home/azureuser/my-node-app
                            git pull origin main
                            npm install
                            pm2 restart app || pm2 start server.js --name my-node-app
                        EOF
                        """
                    }
                }
            }
        } 
    }
}
