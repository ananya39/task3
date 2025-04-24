# 🚀 Strapi Deployment on AWS EC2 using Terraform & Docker

This project demonstrates how to deploy a Strapi backend application on an AWS EC2 instance using:

- ✅ Docker for containerization
- ✅ Terraform for provisioning infrastructure
- ✅ EC2 User Data script for automating Docker setup and app deployment

---

## 📁 Project Structure

strapi-project/
├── my-strapi-app/  
│ ├── config/
│ ├── src/
│ ├── public/
│ ├── Dockerfile  
│ ├── .dockerignore  
│ ├── package.json
│ ├── yarn.lock / package-lock.json
│ └── ...  
│
├── terraform/  
│ ├── main.tf  
│ ├── variables.tf  
│ ├── outputs.tf  
│ ├── provider.tf  
│ └── user_data.sh  
│
└── README.md

---

## 🛠️ Prerequisites

- [Terraform installed](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- AWS Account with IAM User having EC2 full access
- AWS Key Pair (for SSH if needed)
- Docker Hub Account (if you're using your own image)

---

## 🔧 Step-by-Step Deployment Guide

### 1. Update `user_data.sh`

This script runs on EC2 boot to:

- Install Docker
- Pull your Strapi image
- Run it on port `1337`

```bash
#!/bin/bash
yum update -y
amazon-linux-extras enable docker
yum install -y docker
systemctl start docker
systemctl enable docker
usermod -aG docker ec2-user
docker pull ananya117/myapp:latest
docker run -d -p 1337:1337 -e HOST=0.0.0.0 ananya117/myapp:latest
2. Configure Terraform Files
Make sure the following are defined:

main.tf: EC2 instance + security group (open port 1337)

variables.tf: instance type, AMI, key pair, etc.

outputs.tf: display public IP

provider.tf: AWS region + credentials

3. Run Terraform Commands
cd terraform

# Initialize Terraform
terraform init

# Preview what will be created
terraform plan

# Apply the configuration
terraform apply
After a few minutes, Terraform will output your EC2 Public IP.

🌐 Access Strapi
Once the EC2 instance is up and running:


http://<your-ec2-public-ip>:1337
 Make sure the security group allows inbound traffic on port 1337 (TCP).

 Later, you can extend this with:

GitHub Actions to automate Docker builds (CI)

GitHub Actions to trigger Terraform (CD)
```
