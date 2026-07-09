---
title: "Create and configure EC2 backend"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

#### Steps

- Create an **Ubuntu EC2 instance** in us-east-1.
- Configure Security Group rules: SSH 22 from My IP; backend test port 5000 for HTTP API testing; later replace direct HTTP with HTTPS/domain if productionizing.
- Connect using SSH or MobaXterm with username `ubuntu` and the correct private key.
- Install Git, Node.js 22, npm, PM2, PostgreSQL client, and AWS CLI v2.
- Clone/pull the GitHub repository and install backend dependencies.
- Start the backend with PM2 and enable startup persistence.

![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.1-create-configure-ec2-backend/1st-step-create-ec2-Instance-type.png)
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.1-create-configure-ec2-backend/2nd-step-create-ec2-add-security-group-rules.png)
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.1-create-configure-ec2-backend/successfully-launch-instance.png)
```bash
# Server setup
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs git postgresql-client unzip curl
sudo npm install -g pm2

# AWS CLI v2 if apt package is unavailable
cd /tmp
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip -q awscliv2.zip
sudo ./aws/install
aws --version

# Backend runtime
cd ~/daiai-aws-MarketplaceV1/backend
npm install
pm2 start server.js --name marketplace-backend
pm2 save
pm2 startup
```

#### Verification

```bash
pm2 status
pm2 logs marketplace-backend --lines 80
curl http://localhost:5000/health
curl http://localhost:5000/api/products
```
* Test pm2 status
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.1-create-configure-ec2-backend/pm2-status.png)
* Test pm2 logs
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.1-create-configure-ec2-backend/pm2-logs-marketplace-backend.png)

<!-- INSERT FIGURE 5.4: Screenshot of PM2 status and /health response. Mask the public IP if the report is shared publicly. -->
