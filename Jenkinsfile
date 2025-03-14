pipeline {
    agent any 

    environment {
        APP_NAME = "web-app"
        IMAGE_NAME = "priyankabm9914/web-app"
        DEPLOY_USER = "ubuntu"
        DEPLOY_SERVER = "13.201.46.175"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyagowda14/web-app-ci-cd.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh '''
                        echo "Building Docker image..."
                        docker build -t $IMAGE_NAME:latest .

                        echo "Logging into DockerHub..."
                        echo "AWS4@learning" | docker login -u "priyankabm9914" --password-stdin

                        echo "Pushing Docker image to DockerHub..."
                        docker push $IMAGE_NAME:latest
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['deploy-server']) {
                    script {
                        sh '''
                        echo "Deploying application using Docker..."

                        ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_SERVER <<EOF
                            echo "Pulling latest Docker image..."
                            docker pull $IMAGE_NAME:latest

                            echo "Stopping old container if running..."
                            docker stop $APP_NAME || true
                            docker rm $APP_NAME || true

                            echo "Running new container..."
                            docker run -d --name $APP_NAME -p 80:80 --restart always $IMAGE_NAME:latest

                            echo "Cleaning up old images..."
                            docker image prune -f
                        EOF
                        '''
                    }
                }
            }
        }
    }
}
