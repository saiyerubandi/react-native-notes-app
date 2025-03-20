**React Native Notes App - CI/CD Pipeline Deployment**

 Worked on project that sets up a CI/CD pipeline using Jenkins to deploy a React Native Notes App to an AWS EC2 instance following GitOps principles. The pipeline automates:

Code repository updates: Triggering the pipeline on code push.

Infrastructure provisioning: Deploying an EC2 instance via AWS CloudFormation.

Application deployment: Installing dependencies and running the app.

**Technologies**

CI/CD Tool: Jenkins

Infrastructure Provisioning: AWS CloudFormation

Application Hosting: AWS EC2 (Ubuntu)

Build & Deployment: Node.js + PM2


**Ensure you have the following setup:**

AWS Account with permissions to manage EC2 and CloudFormation.

Jenkins Server with AWS CLI and SSH plugin installed.

GitHub Account with a forked repository.

EC2 SSH Key added to Jenkins (ec2-ssh-key).

AWS Credentials stored in Jenkins (aws-credentials).


**Steps to Deploy**

1. Fork the Repository

Fork this repository into your GitHub account.

2. Configure AWS Credentials in Jenkins

Navigate to Jenkins → Manage Jenkins → Credentials.

Add AWS credentials (aws-credentials).

Add EC2 SSH private key (ec2-ssh-key).

3. Set Up Jenkins Pipeline

Create a New Pipeline Job

Go to Jenkins Dashboard → New Item → Pipeline.

Under Pipeline Definition, select Pipeline script from SCM.

Set Repository URL to your forked GitHub repo.

Set Branch to main.

Click Save & Build Now.

4. Jenkinsfile - CI/CD Pipeline

The Jenkinsfile automates the following steps:

5. Infrastructure as Code (CloudFormation)

The ec2-instance.yaml provisions an EC2 instance for the application.

6. Trigger and Verify Deployment

Push code changes to GitHub, which triggers Jenkins pipeline.

Jenkins deploys the EC2 instance and the application.

Check the running application on your EC2 public IP.

7. Access the Application

Get the EC2 Public IP from Jenkins logs or AWS Console.

SSH into the instance:

Check running processes:

Open the application in a browser at:

Submission

Pushed all the changes (Jenkinsfile, ec2_instance.yaml, and README.md) to GitHub repository, kindly find respective files.


Conclusion

This setup ensures a fully automated CI/CD pipeline, utilizing GitOps principles with Jenkins, AWS CloudFormation, and Node.js for deploying a React Native Notes App.
