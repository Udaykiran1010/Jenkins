pipeline {
    agent any

    environment {
        // Define environment variables
        EC2_USER = 'ec2-user' // Replace with your EC2 user
        EC2_HOST = 'ec2-3-109-207-211.ap-south-1.compute.amazonaws.com' // Replace with your EC2 instance's public DNS
        GIT_REPO = 'https://github.com/Udaykiran1010/Jenkins.git' // Replace with your Git repository URL
        SSH_CREDENTIALS_ID ="5189b6c8-436d-44da-8a5b-dc553f214cda"
        // Optionally, define paths for SSH key and Git credentials if needed
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Use Jenkins' SSH Agent plugin to inject SSH credentials
                    sshagent([SSH_CREDENTIALS_ID]) {
                        // Define the SSH command to execute on the EC2 instance
                        def sshCommand = """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'git clone ${GIT_REPO}
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} <<EOF
                        sudo mv /home/${EC2_USER}/Jenkins /var/www/html/
                        sudo systemctl reload httpd
                        EOF
                        """

                        
                        // Execute the SSH command
                        sh "${sshCommand}"
                    }
                }
            }
        }
    }
}
