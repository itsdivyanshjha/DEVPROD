pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/path/to/deploy/'
        SSH_KEY = credentials('my-ssh-key') // Ensure you have added your SSH key in Jenkins credentials
        VM2_IP = '<VM2_Public_IP>'
        USER = 'ec2-user'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/<your-username>/spring-boot-demo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['my-ssh-key']) {
                    sh '''
                    scp target/demo-0.0.1-SNAPSHOT.jar ${USER}@${VM2_IP}:${DEPLOY_DIR}/demo.jar
                    ssh ${USER}@${VM2_IP} "sudo systemctl restart demo"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}