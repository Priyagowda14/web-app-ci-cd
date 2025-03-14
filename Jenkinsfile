pipeline {
    agent any 

    environment {
        REPO_URL = "https://github.com/Priyagowda14/web-app-ci-cd.git"
        DEPLOY_USER = "ubuntu"
        DEPLOY_SERVER = "13.201.46.175"
        INVENTORY_FILE = "inventory"
        PLAYBOOK_FILE = "deploy.yml"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: REPO_URL
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Building the application..."
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "Running tests..."
                '''
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['deploy-server']) {
                    sh '''
                    echo "Running Ansible Playbook for Deployment..."
                    ansible-playbook -i $INVENTORY_FILE $PLAYBOOK_FILE
                    '''
                }
            }
        }
    }
