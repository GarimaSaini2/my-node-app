pipeline {
    agent any

    environment {
        AZURE_VM_USER = 'azureuser'  // Azure VM username
        AZURE_VM_IP = '20.55.27.218' // Azure VM IP
        //SSH_KEY_ID = 'azure-ssh-private-key'  // Correct Jenkins SSH key credentials ID
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
                    if (isUnix()) {
                        sh 'npm install'
                    } else {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm run build'
                    } else {
                        bat 'npm run build'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm test'
                    } else {
                        bat 'npm test'
                    }
                }
            }
        }
        
        stage('Deploy to VM') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: SSH_KEY_ID, keyFileVariable: 'SSH_KEY')]) {  // Fix here
                        // Debugging to check if the SSH key is recognized
                        sh 'echo "SSH Agent is working"'
                        
                        // Try SSH connection with the private key
                        sh "ssh -i $SSH_KEY -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} 'echo SSH Connection Successful'"

                        // Deployment process
                        sh """
                        echo 'Deploying to Azure VM...'
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} << 'EOF'
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


