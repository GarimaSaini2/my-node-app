pipeline {
    agent any

    environment {
        AZURE_VM_USER = 'azureuser'  // Azure VM username
        AZURE_VM_IP = '20.55.27.218' // Azure VM IP
        SSH_KEY_ID = 'azure-ssh-private-key'  // ID for Jenkins SSH key credentials
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
            sshagent(credentials: ['SSH_KEY_ID']) { // Ensure correct credential ID
                // Verify SSH agent is running
                sh 'echo "SSH Agent is working"'
                
                // Test SSH Connection before proceeding
                sh "ssh -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} 'echo SSH Connection Successful' || exit 1"

                // Deploy application
                sh """
                echo 'Deploying to Azure VM...'
                ssh -o StrictHostKeyChecking=no ${AZURE_VM_USER}@${AZURE_VM_IP} << 'EOF'
                    echo "Navigating to app directory..."
                    cd /home/azureuser/my-node-app || { echo "Failed to navigate"; }
                    
                    echo "Pulling latest code..."
                    git pull origin main || { echo "Git pull failed"; }
                    
                    echo "Installing dependencies..."
                    npm install || { echo "npm install failed"; }
                    
                    echo "Restarting application with PM2..."
                    pm2 restart app || pm2 start server.js --name my-node-app || { echo "PM2 restart/start failed"; }

                    echo "Deployment complete!"
                EOF
                """
            }
        }
    }
}

