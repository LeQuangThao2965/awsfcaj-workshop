---
title: "Frontend deployment and S3 product storage"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

The frontend was initially planned for S3 + CloudFront. Because CloudFront creation was blocked by AWS account verification, the actual demo uses **Vercel** for the frontend while keeping **S3** for product assets. The architecture diagram therefore shows **"Vercel or CloudFront"** as frontend delivery options.

This section also covers the most important storage improvement of the workshop: migrating product files from EC2 local storage to **Amazon S3**. Before the migration, uploaded product files were stored in `backend/storage/products` on the EC2 instance. After the migration, new product files are uploaded to S3 under the `products/` prefix.

#### Why S3

- S3 separates product file storage from backend compute.
- S3 avoids losing product files during code pulls, instance replacement, or local disk changes.
- S3 provides scalable object storage for documents, images, thumbnails, and 3D models.
- Files remain private because S3 Block Public Access is enabled and access goes through the backend.

#### Content

- [Deploy frontend to Vercel](5.4.1-deploy-frontend-vercel/)
- [API rewrite and webhook forwarding](5.4.2-api-rewrite-webhook-forwarding/)
- [IAM Role and backend code migration to S3](5.4.3-iam-role-s3-migration/)
- [Testing S3 upload, preview, and download](5.4.4-test-s3-upload-preview-download/)
