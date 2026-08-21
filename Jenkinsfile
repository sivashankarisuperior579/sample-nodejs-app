pipeline {
    agent any

    environment {
        DEPLOY_HOST = credentials('ec2-host')
        DEPLOY_USER = credentials('ec2-username')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sivashankarisuperior579/sample-nodejs-app.git'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} '
                            cd /home/ubuntu/sample-nodejs-app &&
                            git pull origin main &&
                            docker compose up --build -d
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}