pipeline {
    agent any

    environment {
        EC2_USER = 'ec2-user'
        EC2_HOST = 'ec2-3-109-207-211.ap-south-1.compute.amazonaws.com'
        GIT_REPO = 'https://github.com/Udaykiran1010/Jenkins.git'
        SSH_CREDENTIALS_ID = '5189b6c8-436d-44da-8a5b-dc553f214cda'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    sshagent([SSH_CREDENTIALS_ID]) {
                        def sshCommand = "ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'git clone ${GIT_REPO}'"
                        sh "${sshCommand}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent([SSH_CREDENTIALS_ID]) {
                        // Combined command to ensure proper execution
                        def deployCommand = """
                            ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                            sudo mv /home/${EC2_USER}/Jenkins/* /var/www/html/ &&
                            sudo systemctl reload httpd
                            '
                        """
                        sh "${deployCommand}"
                    }
                }
            }
        }
    }
}
