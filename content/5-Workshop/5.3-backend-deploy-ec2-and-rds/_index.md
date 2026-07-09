---
title: "Backend deployment with EC2 and RDS"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

This section describes how the backend is deployed to **Amazon EC2** and connected to **Amazon RDS PostgreSQL**. The backend is the trusted layer that validates JWT tokens, checks roles, communicates with RDS, and streams S3 files only after authorization.

#### Backend services

- **Amazon EC2** hosts the Express backend.
- **PM2** keeps the Node.js process running as `marketplace-backend`.
- **Amazon RDS PostgreSQL** stores relational data.
- **Prisma 7** with `@prisma/adapter-pg` is used for database access.
- The backend exposes `/health`, `/api/auth`, `/api/products`, `/api/categories`, `/api/orders`, `/api/library`, `/api/admin`, `/api/withdrawals`, and `/webhook` routes.

![alt text](../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/rds-summary.png)
#### Content

- [Create and configure EC2 backend](5.3.1-create-configure-ec2-backend/)
- [Configure RDS PostgreSQL and Prisma](5.3.2-configure-rds-postgresql-prisma/)
