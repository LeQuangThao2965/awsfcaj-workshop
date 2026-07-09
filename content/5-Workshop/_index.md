---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Cloud-based Digital Product Marketplace with 3D Preview — DaiMarket

#### Overview

This Workshop section documents the practical deployment and validation of **DaiMarket**, a cloud-based marketplace for digital products. The project sells PDF/Word documents and 3D model files, supports browser-based 3D preview, integrates SePay payment confirmation, and stores product assets on Amazon S3.

The workshop focuses on deploying the application stack to a cost-aware cloud environment. The final working architecture uses **Vercel** for frontend hosting, **Amazon EC2** for the Node.js/Express backend, **Amazon RDS PostgreSQL** for relational data, **Amazon S3** for product file storage, and **IAM Role** permissions for secure S3 access from EC2.

- **Frontend:** React + Vite hosted on Vercel. CloudFront remains an alternative target, but account verification prevented using it during the demo period.
- **Backend:** Node.js + Express + Prisma 7 running on an EC2 instance through PM2.
- **Database:** Amazon RDS PostgreSQL for users, roles, products, categories, orders, payment methods, and library ownership.
- **Object storage:** Amazon S3 bucket `marketplace-frontend-thao`, using the `products/` prefix for private product assets.
- **Security:** IAM Role `marketplace-ec2-s3-role` attached to EC2, with least-privilege access to the S3 `products/` prefix.
- **Payment:** SePay webhook endpoint for real-time transfer notification and order status updates.

<!-- INSERT FIGURE 5.0: Updated AWS architecture diagram showing Vercel or CloudFront, EC2 Backend, RDS PostgreSQL, S3 Product Assets, IAM Role, and SePay webhook. -->

#### Content

1. [Workshop overview](5.1-Workshop-overview/)
2. [Prerequisite](5.2-prerequisite/)
3. [Backend deployment with EC2 and RDS](5.3-backend-deploy-ec2-and-rds/)
4. [Frontend deployment and S3 product storage](5.4-frontend-deploy-and-s3-storage/)
5. [Payment, admin validation, and system testing](5.5-payment-admin-system-testing/)
6. [Cleanup, cost control, and next steps](5.6-cleanup-cost-next-steps/)
