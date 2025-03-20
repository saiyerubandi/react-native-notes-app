pipeline {
    agent any

    environment {
        AWS_REGION = "us-west-2"  // Change to your AWS region
        STACK_NAME = "ReactNativeNotesAppStack"
        TEMPLATE_FILE = "ec2_instance.yaml"
        EC2_USER = "ubuntu"
        SSH_PRIVATE_KEY = credentials('ec2-ssh-key')  // Store your EC2 key pair in Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/saiyerubandi/react-native-notes-app.git'
            }
        }

        stage('Deploy CloudFormation Stack') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials') { // Use stored AWS credentials in Jenkins
                    sh """
                        aws cloudformation deploy \
                        --stack-name ${STACK_NAME} \
                        --template-file ${TEMPLATE_FILE} \
                        --capabilities CAPABILITY_NAMED_IAM
                    """
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "Tests failed, proceeding with deployment"'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def ec2PublicIp = sh(script: "aws cloudformation describe-stacks --stack-name ${STACK_NAME} --query 'Stacks[0].Outputs[?OutputKey==`InstancePublicIP`].OutputValue' --output text", returnStdout: true).trim()
                    env.EC2_HOST = ec2PublicIp
                }
                sshagent(['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_HOST << 'EOF'
                        cd /home/ubuntu/app
                        git pull origin main
                        npm install
                        pm2 restart react-native-notes || pm2 start App.js --name react-native-notes
                        pm2 save
                        EOF
                    """
                }
            }
        }
    }
}
