---
title: "IAM Role and backend code migration to S3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

#### Create the IAM Role for EC2

1. Create the role `marketplace-ec2-s3-role` with the **EC2** trusted service.
2. Attach an inline policy scoped to the product prefix only:
   - `s3:ListBucket` on `arn:aws:s3:::marketplace-frontend-thao` with condition `s3:prefix = products/*`
   - `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject` on `arn:aws:s3:::marketplace-frontend-thao/products/*`
3. Attach the role to the instance via **EC2 > Actions > Security > Modify IAM role**.
4. Keep **S3 Block Public Access** enabled — the backend accesses S3 through the role, no public objects.

<!-- INSERT FIGURE 5.8: Screenshot of the EC2 instance Security tab showing marketplace-ec2-s3-role attached, and the inline IAM policy. -->
![alt text](../../../images/5-Workshop/5.4-frontend-deploy-and-s3-storage/5.4.3-iam-role-s3-migration/EC2-IAMRole-Censored.png)

#### Verify the role from EC2 (IMDSv2)

```bash
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -s -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/iam/security-credentials/
# Expected output: marketplace-ec2-s3-role
```

#### Test S3 operations with AWS CLI

```bash
echo "s3 role test" > /tmp/s3-test.txt
aws s3 cp /tmp/s3-test.txt s3://marketplace-frontend-thao/products/_test/s3-test.txt --region us-east-1
aws s3 ls s3://marketplace-frontend-thao/products/_test/ --region us-east-1
aws s3 rm s3://marketplace-frontend-thao/products/_test/s3-test.txt --region us-east-1
```

#### Backend code changes

| File | Change |
| --- | --- |
| `src/config/s3.js` | New helper using `@aws-sdk/client-s3` (upload, get, delete) |
| `src/middlewares/upload.middleware.js` | Multer `diskStorage` → `memoryStorage` |
| `src/controllers/product.controller.js` | Upload product file/thumbnail/model to S3; stream from S3 |
| `src/controllers/library.controller.js` | `GetObject` download after ownership check |
| `package.json` | Added `@aws-sdk/client-s3` dependency |

#### Storage key convention

```text
fileUrl          = <uuid>.docx        -> storageKey = products/<uuid>.docx
thumbnail        = <uuid>.png         -> storageKey = products/<uuid>.png
previewModelUrl  = <uuid>.glb         -> storageKey = products/<uuid>.glb
```

#### Migrate legacy files

```bash
cd ~/daiai-aws-MarketplaceV1/backend
aws s3 sync storage/products/ s3://marketplace-frontend-thao/products/ --region us-east-1
```
