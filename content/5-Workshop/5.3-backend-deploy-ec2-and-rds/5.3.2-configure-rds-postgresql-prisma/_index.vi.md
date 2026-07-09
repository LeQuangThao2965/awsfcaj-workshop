---
title: "Cấu hình RDS PostgreSQL và Prisma"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### Cấu hình RDS

- Engine: **PostgreSQL**.
- Deployment: **Single-AZ** DB instance để tối ưu chi phí.
- Instance class: **db.t4g.micro** cho môi trường demo.
- Storage: gp3, 20 GiB, tắt autoscaling nếu chưa cần.
- Public access: **No**. RDS chỉ cho EC2 Security Group truy cập.
- Database name: `marketplace`.
- Encryption: default AWS/RDS key.

#### Deploy Prisma

Vì project dùng **Prisma 7**, runtime cần đọc `DATABASE_URL` thông qua adapter config, còn migration dùng Prisma CLI script. Backend đã được chỉnh để load dotenv và dùng `PrismaPg` với SSL setting phù hợp RDS.

```bash
cd ~/daiai-aws-MarketplaceV1/backend
npm run generate
node -r dotenv/config ./node_modules/prisma/build/index.js migrate deploy
node prisma/seed-required.js
```

#### Seed data

- Role: `admin`, `buyer`, `seller`.
- Payment method: MoMo, ZaloPay, MB Bank, VNPay.
- Category có group `DOCUMENT` và `MODEL_3D`.
- Seed production chỉ dùng **upsert**, không xóa Product, User, Order hoặc OrderItem.

#### Kiểm tra

```bash
psql -h <RDS-ENDPOINT> -U postgres -d marketplace -p 5432 -c 'SELECT * FROM "Role";'
psql -h <RDS-ENDPOINT> -U postgres -d marketplace -p 5432 -c 'SELECT id, name, "group" FROM "Category" ORDER BY "group", name;'
```

<!-- INSERT FIGURE 5.5: Ảnh RDS summary và Security Group cho phép PostgreSQL từ EC2 SG. -->
<!-- INSERT FIGURE 5.6: Output psql cho seed data Role và Category. -->
### Các bước tạo RDS
# 1.Engine Settings
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/1st-step-create-rds-settings-engine.png)
# 2.Availability and durability
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/2nd-step-rds-create-settings-availability.png)
# 3.Settings
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/3rd-step-rds-create-settings.png)
# 4.Credentials Settings
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/4th-step-rds-create-settings-credentials.png)
# 5.Instance Configs
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/5th-step-rds-create-instance-config.png)
# 6.Storage settings
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/7th-step-rds-create-storage-and-config.png)
# 7.Connectivity part 1
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/8th-step-rds-create-connectivity-first-part.png)
# 8.Connectivity part 2
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/9th-step-rds-create-connectivity-second-part.png)
# 9.Connectivity part 3
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/10th-step-rds-create-connectivity-third-part.png)
# 10.Monitoring settings
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/11th-step-rds-create-monitoring.png)
# 11.Additional config part 1
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/12th-step-rds-create-additionals-config-first-part.png)
# 12.Additional config part 2
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/13th-step-rds-create-additionals-config-second-part.png)
# 13. Estimated monthly costs
![alt text](../../../../images/5-Workshop/5.3-backend-deploy-ec2-and-rds/5.3.2-configure-rds-postgresql-prisma/last-of-all-Estimated-monthly-costs.png)