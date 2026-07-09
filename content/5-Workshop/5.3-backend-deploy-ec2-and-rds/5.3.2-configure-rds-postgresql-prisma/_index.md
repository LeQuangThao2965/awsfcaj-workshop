---
title: "Configure RDS PostgreSQL and Prisma"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### RDS configuration

- Engine: **PostgreSQL**.
- Deployment: **Single-AZ** DB instance for cost optimization.
- Instance class: **db.t4g.micro** for the demo environment.
- Storage: gp3, 20 GiB, autoscaling disabled unless needed.
- Public access: **No**. RDS is accessed from the EC2 backend Security Group.
- Database name: `marketplace`.
- Encryption: default AWS/RDS key.

#### Prisma deployment

Because the project uses **Prisma 7**, the runtime must read `DATABASE_URL` through the adapter configuration, while migrations use Prisma CLI scripts. The backend was adjusted to load dotenv and use `PrismaPg` with SSL settings for RDS.

```bash
cd ~/daiai-aws-MarketplaceV1/backend
npm run generate
node -r dotenv/config ./node_modules/prisma/build/index.js migrate deploy
node prisma/seed-required.js
```

#### Seed data

- Roles: `admin`, `buyer`, `seller`.
- Payment methods: MoMo, ZaloPay, MB Bank, VNPay.
- Categories with group values such as `DOCUMENT` and `MODEL_3D`.
- The production seed uses **upsert only** and must not delete Product, User, Order, or OrderItem records.

#### Verification

```bash
psql -h <RDS-ENDPOINT> -U postgres -d marketplace -p 5432 -c 'SELECT * FROM "Role";'
psql -h <RDS-ENDPOINT> -U postgres -d marketplace -p 5432 -c 'SELECT id, name, "group" FROM "Category" ORDER BY "group", name;'
```

<!-- INSERT FIGURE 5.5: RDS instance summary and Security Group rule showing PostgreSQL access from the EC2 Security Group only. -->
<!-- INSERT FIGURE 5.6: psql query output for Role and Category seed data. -->
### Steps to create RDS
# 1.Engine Settings
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/1st-step-create-rds-settings-engine.png)
# 2.Availability and durability
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/2nd-step-rds-create-settings-availability.png)
# 3.Settings
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/3rd-step-rds-create-settings.png)
# 4.Credentials Settings
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/4th-step-rds-create-settings-credentials.png)
# 5.Instance Configs
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/5th-step-rds-create-instance-config.png)
# 6.Storage settings
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/7th-step-rds-create-storage-and-config.png)
# 7.Connectivity part 1
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/8th-step-rds-create-connectivity-first-part.png)
# 8.Connectivity part 2
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/9th-step-rds-create-connectivity-second-part.png)
# 9.Connectivity part 3
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/10th-step-rds-create-connectivity-third-part.png)
# 10.Monitoring settings
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/11th-step-rds-create-monitoring.png)
# 11.Additional config part 1
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/12th-step-rds-create-additionals-config-first-part.png)
# 12.Additional config part 2
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/13th-step-rds-create-additionals-config-second-part.png)
# 13. Estimated monthly costs
![alt text](../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/last-of-all-Estimated-monthly-costs.png)