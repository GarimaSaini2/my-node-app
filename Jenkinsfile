pipeline {
    agent any

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
                        sh 'npm install'  // For Linux/macOS
                    } else {
                        bat 'npm install'  // For Windows (uses CMD)
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

        stage('Deploy to Azure') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'az webapp up --name my-node-app-1 --resource-group devops-assignment-rg'
                    } else {
                        bat 'az webapp up --name my-node-app-1 --resource-group devops-assignment-rg'
                    }
                }
            }
        }
    }
}

                
    
