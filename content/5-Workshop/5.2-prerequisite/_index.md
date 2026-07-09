---
title: "Prerequisite"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Required accounts and tools

- AWS account in the **us-east-1** region.
- GitHub repository containing frontend and backend source code.
- Vercel account connected to GitHub for frontend deployment.
- SSH key pair for EC2 access, or another approved EC2 access method.
- Node.js 22 on the backend server, npm, PM2, Git, PostgreSQL client, and AWS CLI v2.
- A PostgreSQL database on Amazon RDS and a private S3 bucket/prefix for product assets.
- SePay API key and bank transfer configuration for payment webhook testing.

#### Environment variables

{{% notice note %}}
A friendly Reminder: Do not commit dotenv file to Git or paste them into a AI conversation. Database passwords, JWT secrets, SePay API keys, and bank account numbers below are masked/censored on purpose.
{{% /notice %}}

```text
PORT=5000
NODE_ENV=production
DATABASE_URL=postgresql://postgres:<PASSWORD>@<RDS-ENDPOINT>:5432/marketplace
JWT_SECRET=<must be long and secure>
CORS_ORIGIN=<example: https://daiai-aws.vercel.app>
SEED_MOCK_DATA=false
AWS_REGION=us-east-1
S3_BUCKET_NAME=<bucket-name>
S3_PRODUCT_PREFIX=products
SEPAY_API_KEY=<Where to get: SePay Dashboard -> Settings -> API Key>
BANK_BIN=<bank-bin>
BANK_NAME=MB Bank
BANK_ACCOUNT_NO=<masked>
BANK_ACCOUNT_NAME=<masked>
```

#### IAM permission model

The EC2 instance uses an **IAM Role** instead of static AWS access keys. This avoids storing long-term AWS credentials on the server. The role is limited to the `products/` prefix of the selected S3 bucket.
![iam role gắn vào s3](../../images/5-Workshop/5.2-prerequisite/IAMrole_marketplace-ec2-s3-role.png)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListBucketProductPrefix",
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::marketplace-frontend-thao",
      "Condition": { "StringLike": { "s3:prefix": ["products/*"] } }
    },
    {
      "Sid": "ManageProductObjects",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::marketplace-frontend-thao/products/*"
    }
  ]
}
```

* S3 also needs to have Block All Public Access enabled if it's not already.
![s3 block all public access](../../images/5-Workshop/5.2-prerequisite/block-all-public-access-for-s3.png)
<!-- INSERT FIGURE 5.2: Screenshot of EC2 instance Security tab showing IAM Role = marketplace-ec2-s3-role. -->
<!-- INSERT FIGURE 5.3: Screenshot of S3 bucket with Block all public access enabled. -->
