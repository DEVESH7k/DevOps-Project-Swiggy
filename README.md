# 🚀 **DevOps Real-time Project: Swiggy Clone App Deployment**

In this **real-time DevOps project**, I demonstrate how to **deploy a Swiggy Clone App** using various modern tools and services in the DevOps ecosystem.
## 🛠️ Tools & Services Used:

1. **Terraform** ![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white)
2. **GitHub** ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)
3. **Jenkins** ![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white)
4. **SonarQube** ![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=flat-square&logo=sonarqube&logoColor=white)
5. **OWASP** ![OWASP](https://img.shields.io/badge/OWASP-000000?style=flat-square&logo=owasp&logoColor=white)
6. **Trivy** ![Trivy](https://img.shields.io/badge/Trivy-00979D?style=flat-square&logo=trivy&logoColor=white)
7. **Docker & DockerHub** ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![DockerHub](https://img.shields.io/badge/DockerHub-2496ED?style=flat-square&logo=docker&logoColor=white)

--

### 📂 Terraform Script Repository: [**Terraform Script for Swiggy Clone App**](https://github.com/DEVESH7k/Terraform-Script-Swiggy.git)



Search
Write

Devesh Khatik
Building a Real-Time Swiggy Clone Application with Jenkins, Docker, SonarQube, and More
Devesh Khatik
Devesh Khatik
8 min read
·
Just now





Zoom image will be displayed

Complete DevOps CI/CD Pipeline: From Infrastructure to Deployment
In today’s fast-paced development environment, implementing a robust CI/CD pipeline is crucial for successful application deployment. This comprehensive guide walks you through building a complete DevOps pipeline for deploying a Swiggy-based Node.js application using industry-standard tools and practices.

🎯 Project Overview
This project demonstrates a real-world DevOps implementation that includes:

Infrastructure automation using Terraform
Continuous Integration with Jenkins
Code quality analysis with SonarQube
Security scanning with OWASP and Trivy
Containerization with Docker
Automated deployment and monitoring
🛠️ Technology Stack
Infrastructure & Automation
Terraform: Infrastructure as Code (IaC)
AWS EC2: Cloud computing platform
AWS Security Groups: Network security
CI/CD Pipeline
Jenkins: Automation server for CI/CD
GitHub: Source code management
Docker: Containerization platform
Docker Hub: Container registry
Code Quality & Security
SonarQube: Code quality analysis
OWASP Dependency Check: Security vulnerability scanning
Trivy: File system and image security scanning
Application Stack
Node.js: Runtime environment
npm: Package manager
🏗️ Architecture Overview
The pipeline follows these stages:

Infrastructure Creation → Terraform provisions AWS resources
Source Code Management → GitHub repository integration
Code Quality Analysis → SonarQube performs static analysis
Security Scanning → OWASP and Trivy scan for vulnerabilities
Build Process → Docker image creation
Registry Push → Docker Hub deployment
Container Deployment → Application deployment in Docker container
📋 Prerequisites
Before starting, ensure you have:

AWS account with appropriate permissions
GitHub account
Docker Hub account
Basic understanding of DevOps concepts
VS Code or similar IDE
🚀 Step-by-Step Implementation
Phase 1: Infrastructure Setup with Terraform
1.1 AWS IAM User Configuration
First, create an IAM user with administrator permissions:

Zoom image will be displayed
Zoom image will be displayed
# Navigate to AWS Console → IAM → Users → Create User
# Username: swiggy(or your preferred name)
# Attach AdministratorAccess policy
# Generate Access Keys and Secret Keys
1.2 Terraform Configuration Files
Create three essential files:

https://github.com/DEVESH7k/Terraform-Script-Swiggy.git
main.tf — Main infrastructure configuration:

Zoom image will be displayed

# Security Group Configuration
resource "aws_security_group" "project-sg" {
  name = "project-sg"
  
  # Ingress rules for various services
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 9000
    to_port     = 9000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "project-sg"
  }
}
# EC2 Instance Configuration
resource "aws_instance" "castro" {
  ami                    = "ami-0c02fb55956c7d316"  # Update with your region's AMI
  instance_type          = "t2.large"
  key_name              = "swiggy"  # Your key pair name
  vpc_security_group_ids = [aws_security_group.project-sg.id]
  
  user_data = file("resource.sh")
  
  root_block_device {
    volume_size = 30
  }
  
  tags = {
    Name = "swiggy"
  }
}
provider.tf — Provider configuration:

provider "aws" {
  region = "ap-south-1"  # Mumbai region
}
resource.sh — Installation script:

#!/bin/bash
# Update system
apt update -y
# Install Java 17
apt install openjdk-17-jdk -y
# Install Jenkins
wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
apt-get update -y
apt-get install jenkins -y
systemctl start jenkins
systemctl enable jenkins
# Install Docker
apt-get update -y
apt-get install ca-certificates curl gnupg -y
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update -y
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
systemctl start docker
systemctl enable docker
usermod -aG docker jenkins
# Install Trivy
apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | tee -a /etc/apt/sources.list.d/trivy.list
apt-get update -y
apt-get install trivy -y
1.3 Infrastructure Deployment
Execute Terraform commands:

# Configure AWS CLI
aws configure
# Provide Access Key, Secret Key, Region (ap-southeast-2), and output format (json)
# Initialize Terraform
terraform init
# Plan the deployment
terraform plan
# Apply the configuration
terraform apply --auto-approve
Phase 2: Jenkins Configuration
2.1 Initial Jenkins Setup
Access Jenkins at http://your-ec2-public-ip:8080
Get the initial admin password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Install suggested plugins
Create admin user
Zoom image will be displayed

2.2 Plugin Installation
Install the following plugins via Manage Jenkins → Plugins → Available:

Eclipse Temurin Installer
Pipeline Stage View
SonarQube Scanner
NodeJS
OWASP Dependency Check
Docker (all Docker-related plugins)
Zoom image will be displayed

2.3 Tool Configuration
Navigate to Manage Jenkins → Tools and configure:

JDK Configuration:

Name: jdk17
Install automatically: ✅
Version: jdk-17.0.1+12
SonarQube Scanner:

Name: sonar-scanner
Install automatically: ✅
Version: Latest
NodeJS:

Name: node23
Install automatically: ✅
Version: 23.0.0
Docker:

Name: docker
Install automatically: ✅
Version: Latest
OWASP Dependency Check:

Name: DP-Check
Install automatically: ✅
Version: 10.0.3
Zoom image will be displayed

Phase 3: SonarQube Configuration
3.1 SonarQube Access and Setup
Access SonarQube at http://your-ec2-public-ip:9000
Default credentials: admin/admin
Update password when prompted
3.2 Token Generation
Go to Administration → Security → Users
Click on tokens for admin user
Generate new token with 30-day expiration
Copy the token for Jenkins configuration
3.3 Jenkins-SonarQube Integration
Configure SonarQube credentials in Jenkins:

Manage Jenkins → Security → Credentials
Add Credentials:
Kind: Secret text
Secret: [SonarQube token]
ID: sonar-token
Zoom image will be displayed

Configure SonarQube server in Jenkins:

Manage Jenkins → System
SonarQube servers:
Name: sonar-server
Server URL: http://your-ec2-public-ip:9000
Authentication token: Select sonar-token
3.4 Webhook Configuration
In SonarQube:

Administration → Configuration → Webhooks
Create webhook:
Name: jenkins
URL: http://your-ec2-public-ip:8080/sonarqube-webhook/
Zoom image will be displayed

Phase 4: Docker Hub Integration
4.1 Docker Hub Credentials
Configure Docker Hub credentials in Jenkins:

Manage Jenkins → Security → Credentials
Add Credentials:
Kind: Username with password
Username: [Your Docker Hub username]
Password: [Your Docker Hub password]
ID: docker-credentials
Zoom image will be displayed

Phase 5: Pipeline Creation
5.1 Jenkins Pipeline Script
Create a new Jenkins pipeline job with the following script:

https://github.com/DEVESH7k/DevOps-Project-Swiggy.git
pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node23'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git 'https://github.com/DEVESH7k/DevOps-Project-Swiggy.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Swiggy \
                    -Dsonar.projectKey=Swiggy '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker'){   
                       sh "docker build -t swiggy ."
                       sh "docker tag swiggy deveshkhatik007/swiggy:latest "
                       sh "docker push deveshkhatik007/swiggy:latest "
                    }
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image deveshkhatik007/swiggy:latest > trivy.txt" 
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name swiggy -p 3000:3000 deveshkhatik007/swiggy:latest'
            }
        }
    }
}
Zoom image will be displayed

5.2 Dockerfile Configuration
Ensure your repository contains a Dockerfile:

FROM node:23
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
Phase 6: Pipeline Execution and Monitoring
6.1 Build Execution
Trigger the build by clicking Build Now
Monitor the pipeline execution through the stage view
Each stage will execute sequentially
6.2 Expected Execution Times
Tool Installation: 1–2 minutes
Code Checkout: 30 seconds
SonarQube Analysis: 1–2 minutes
OWASP Scan: 30–35 minutes (most time-consuming)
Docker Build & Push: 3–5 minutes
Deployment: 1 minute
6.3 Verification Steps
Check Docker Images:

docker images
Verify Running Containers:

docker ps
Zoom image will be displayed

Access Application: Navigate to http://your-ec2-public-ip:3000

📊 Monitoring and Reports
SonarQube Analysis Results
Access detailed code quality reports in SonarQube dashboard
Review security hotspots, bugs, and code smells
Zoom image will be displayed

Monitor code coverage and maintainability metrics
Zoom image will be displayed

Final Output in Docker container:

Zoom image will be displayed
Zoom image will be displayed
Security Scanning Reports
OWASP: Dependency vulnerability reports
Trivy: File system and container image security analysis
Destroy all Instances using Terraform:
Zoom image will be displayed

🎯 Key Benefits Achieved
Automated Infrastructure: Terraform ensures consistent, reproducible infrastructure
Quality Assurance: SonarQube maintains code quality standards
Security: Multiple security scanning layers
Containerization: Docker ensures consistent deployment environments
CI/CD Automation: Jenkins automates the entire deployment pipeline
🔧 Troubleshooting Common Issues
Build Failures
Verify all tool configurations match pipeline script names
Check AWS security group ports are properly opened
Ensure Docker Hub credentials are correctly configured
Performance Optimization
The OWASP scan is time-intensive but crucial for security
Consider running security scans on scheduled builds rather than every commit
Use Docker layer caching to speed up builds
🚀 Next Steps
This pipeline can be extended with:

Kubernetes Deployment: Container orchestration
Monitoring Stack: Prometheus and Grafana integration
Notification Systems: Slack/Email integration
Multi-environment Deployments: Dev/Staging/Production workflows
💡 Best Practices Implemented
Infrastructure as Code: Version-controlled infrastructure
Security First: Multiple security scanning layers
Quality Gates: Automated quality checks
Container Security: Image scanning before deployment
Automated Testing: Integrated into CI/CD pipeline
🎉 Conclusion
This comprehensive DevOps pipeline demonstrates how to implement enterprise-grade CI/CD practices. By combining infrastructure automation, continuous integration, security scanning, and containerization, we’ve created a robust deployment pipeline that ensures code quality, security, and reliability.

The project showcases real-world DevOps practices that can be adapted for various application types and scales. Whether you’re working on a small startup project or enterprise application, these principles and tools provide a solid foundation for modern software delivery.

Remember to terminate your AWS resources after completing the project to avoid unnecessary charges. You can always recreate the infrastructure using the Terraform scripts when needed.

Tags: #DevOps #Jenkins #Docker #AWS #Terraform #SonarQube #CI/CD #NodeJS #Security #Automation

Connect with me on LinkedIn

https://www.linkedin.com/in/deveshkhatik/

to discuss more about DevOps practices and share your implementation experiences!



