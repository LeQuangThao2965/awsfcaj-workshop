---
title: "Workshop overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Project purpose

**DaiMarket** is a marketplace platform for digital assets such as PDF/Word documents and 3D models. The system lets users browse products, view product details, preview 3D models or sample documents, purchase products, and access owned items from a personal library.

<img alt="hosted zone" src="../../images/5-Workshop/5.1-workshop-overview/project_architecture.png">

#### Implemented architecture

| Layer | Implemented service / technology | Role in the system |
| --- | --- | --- |
| Frontend | Vercel / React + Vite | Serves the web UI, SPA routing, and frontend build. |
| API | Amazon EC2 / Node.js, Express, PM2 | Runs backend routes for auth, products, admin, orders, webhooks, and library. |
| Database | Amazon RDS / PostgreSQL | Stores users, roles, categories, products, orders, order items, payment methods, seller data, and purchase state. |
| Storage | Amazon S3 | Stores product assets such as thumbnails, documents, original files, and 3D preview models. |
| Security | IAM Role | Allows EC2 to PutObject, GetObject, DeleteObject, and ListBucket only within the configured S3 product prefix. |
| Payment | SePay webhook | Receives transfer notifications and updates orders from PENDING to SUCCESS. |

#### Architecture notes

- The planned (target) architecture originally included **CloudFront, Route 53, WAF**, and optionally **Secrets Manager**. In the actual demo, **Vercel replaces CloudFront** for frontend hosting because CloudFront creation was blocked by AWS account verification.
- The backend remains on AWS EC2 and uses **RDS + S3 + IAM Role**, which are the most important AWS services for the current deployment.
- **S3 Block Public Access** remains enabled. Product files are not public; the backend checks user ownership before streaming them to the browser.
- The current S3 access pattern streams files through the backend. A future production improvement is to generate short-lived **S3 presigned URLs** after verifying ownership.

<!-- INSERT FIGURE 5.1: Implemented cloud architecture using Vercel or CloudFront for frontend delivery, EC2 backend, RDS PostgreSQL, private S3 product storage, IAM Role, and SePay webhook. -->
