---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cloud-based Digital Product Marketplace with 3D Preview
## DaiMarket / AWS Marketplace DaiAI — A digital-product marketplace with integrated 3D preview, real-time payment, and AWS-based storage

### 1. Executive Summary
*About the project* <br>
**DaiMarket / AWS Marketplace DaiAI** is a digital-product marketplace focused on resources such as PDF/Word documents, templates, ebooks, 3D models, and design assets. The system allows users to register, log in, search products, view details, preview selected content, make payments, and access purchased products after the order is confirmed successfully.

The key feature of the project is an integrated 3D preview experience directly in the browser. For 3D products, buyers can inspect the model before purchasing; for documents, the system supports a limited preview so buyers can evaluate the content before deciding to pay.

In terms of deployment, the Frontend uses React + Vite and is currently deployed on Vercel to get a stable HTTPS URL. The Node.js + Express Backend runs on Amazon EC2 with PM2 and connects to Amazon RDS PostgreSQL through Prisma 7. Product files, thumbnails, and preview models are stored on Amazon S3 under the `products/` prefix, while EC2 accesses S3 through an IAM Role following the least-privilege principle.

*Project objectives*
<br>&emsp;- Apply the AWS knowledge learned to a real web application with a complete frontend, backend, database, and file storage.
<br>&emsp;- Separate compute and storage: EC2 handles business logic, RDS stores relational data, S3 stores product assets.
<br>&emsp;- Build the marketplace flows: buyer, seller/admin, product management, order, library, and payment webhook.
<br>&emsp;- Experiment with access security using JWT, IAM Role, and backend authorization before allowing product file downloads.
<br>&emsp;- Create a foundation that can expand to CloudFront, Route 53, CloudWatch, CI/CD, and monitoring in a later phase.


### 2. Problem Statement
*Current problem*

Digital-product marketplaces often face three main groups of problems: a preview experience that is not visual enough, file storage on the application server that is hard to scale, and a manual payment verification process that is slow. For products such as 3D models, if buyers can only look at a cover image or a text description, it is very hard to properly assess product quality.

If product files are stored directly on the backend server's disk, the system depends on EC2 capacity, is hard to back up, hard to scale, and risks losing files when deploying or changing source code. In practice, during trial deployment, storing files at `backend/storage/products` caused risks when git pull/stash created a mismatch between database metadata and the physical files.

In addition, a manual payment process does not suit the marketplace model. The system needs a mechanism that automatically receives transaction webhooks, reconciles order codes, updates order status, and grants file access to the buyer after a successful payment.

*Proposed solution*
<br>&emsp;- The React/Vite Frontend provides the digital-product marketplace UI, 3D Viewer, admin pages, and personal library.
<br>&emsp;- The Node.js/Express Backend handles the API, JWT authentication, authorization, product upload, orders, webhooks, and product ownership checks.
<br>&emsp;- Amazon RDS PostgreSQL stores users, roles, categories, product metadata, orders, order items, payment methods, and transaction status.
<br>&emsp;- Amazon S3 stores product files, thumbnails, and preview models; the backend uploads/streams files from S3 instead of storing them long-term on EC2.
<br>&emsp;- EC2 accesses S3 through the IAM Role `marketplace-ec2-s3-role`, with no hardcoded access keys in the source code.
<br>&emsp;- The SePay webhook automatically updates orders when a transaction notification is received.


### 3. Solution Architecture
The architecture is designed with clearly separated layers: the UI layer, API layer, relational data layer, object storage layer, and the security/permission management layer. In the current deployment, the frontend is hosted on Vercel; the target design can replace or complement it with CloudFront once the AWS account is verified and ready to use a CDN.

<!--
{{% notice warning %}}
**Figure 2.1 not available yet** — DaiMarket AWS architecture diagram: shows Vercel or CloudFront for the frontend, EC2 backend, RDS PostgreSQL, S3 product assets, IAM Role, CloudWatch, and the SePay webhook. Purpose: gives the reader a full picture of the system components and the data flows between them.
{{% /notice %}}
-->
![project architecture](../images/5-Workshop/5.1-workshop-overview/project_architecture.png)

Overall system flow: users access the frontend; the frontend calls the API via `/api`; requests are forwarded to the EC2 backend; the backend handles business logic, queries RDS, and uploads or streams files from S3; when a payment occurs, SePay sends a webhook so the backend can confirm and update the order status.

***Deployed components***

Component | Current implementation | Role in the system
|---|---|---|
|Frontend | React + Vite deployed on Vercel | Provides the user interface, product listing, admin dashboard, 3D viewer, and library.|
|Backend API | Node.js + Express on Amazon EC2, managed by PM2 | Handles the REST API, auth, roles, products, orders, the SePay webhook, and S3 streaming.|
|Database | Amazon RDS PostgreSQL | Stores relational data: users, roles, categories, product metadata, orders, payments.|
|Product Storage | Amazon S3 bucket `marketplace-frontend-thao`, prefix `products/` | Stores product files, thumbnails, preview models; the backend accesses it via IAM Role.|
|IAM | `marketplace-ec2-s3-role` | Grants PutObject/GetObject/DeleteObject/ListBucket permissions limited to the `products/` prefix.|
|Payment | SePay webhook | Receives transaction notifications, reconciles the order code, and updates the order to SUCCESS.|
|Monitoring | PM2 logs, AWS Budget; CloudWatch as a future extension | Tracks backend errors and costs; can expand to logs/alarms.|

<!--
{{% notice note %}}
**Notes on target architecture vs. actual implementation**
<br>&emsp;- The target diagram includes Route 53, ACM, CloudFront, and WAF. These components suit production, but not all of them are deployed in the current demo.
<br>&emsp;- Vercel currently replaces the frontend hosting/CDN role during the demo phase because CloudFront is blocked by the AWS account verification step.
<br>&emsp;- S3 has been repurposed correctly for product asset storage. The bucket should remain private; users must not read objects directly without the backend checking permissions first.
<br>&emsp;- The backend currently streams files from S3 to the client after verifying ownership. A later phase can switch to presigned URLs to reduce the load on EC2.
{{% /notice %}}
-->

### 4. Technical Implementation
*4.1. Backend and database*
<br>&emsp;- The Node.js + Express backend is deployed on EC2 Ubuntu and managed by PM2.
<br>&emsp;- Prisma 7 connects to RDS PostgreSQL through `@prisma/adapter-pg`.
<br>&emsp;- The RDS connection error through Node/Prisma was resolved by configuring SSL in the Prisma adapter.
<br>&emsp;- The required seed uses upsert to create the admin/buyer/seller roles, payment methods, and categories without deleting real data.
<br>&emsp;- The main APIs include auth, admin, products, categories, seller applications, orders, webhook, withdrawals, and library.

*4.2. Product storage on Amazon S3*
<br>&emsp;- Initially product files were stored in `backend/storage/products` on EC2, but this approach was unstable across deployments and unsuitable for scaling.
<br>&emsp;- The backend switched to `multer.memoryStorage` to receive files temporarily in RAM, then uploads the buffer to S3 using the AWS SDK.
<br>&emsp;- The database stores `fileUrl` as a filename and `storageKey` as `products/<uuid>.<ext>` to retrieve files from S3.
<br>&emsp;- When a user views a thumbnail, previews a model, or downloads a file in the library, the backend fetches the object from S3 and streams it to the browser.
<br>&emsp;- EC2 has the IAM Role `marketplace-ec2-s3-role` attached, and PutObject, ListObject, and DeleteObject were successfully tested with the S3 `products/` prefix.

*4.3. Frontend and routing*
<br>&emsp;- The React/Vite frontend is deployed on Vercel.
<br>&emsp;- SPA routing is handled by rewrites so routes such as `/login`, `/register`, and `/products` can be accessed directly.
<br>&emsp;- Vercel rewrites `/api/*` to the EC2 backend to avoid mixed-content errors and let the frontend call the API by relative path.
<br>&emsp;- Hardcoded localhost URLs in the frontend API code were reviewed and fixed to `/api/...` to fit the deployed environment.

*4.4. Payment and post-purchase access*
<br>&emsp;- Orders are created in PENDING status after the buyer checks out.
<br>&emsp;- The SePay webhook receives the transfer message, extracts the `DAIMxxxxxxxx` order code, finds the matching order, and updates PENDING to SUCCESS.
<br>&emsp;- The library only displays/downloads products when the user has a SUCCESS order containing that product.
<br>&emsp;- Downloads and previews go through the backend to verify the JWT and product ownership before streaming files from S3.


### 5. Timeline & Milestones
Milestone | Main work | Result
|---|---|---|
|Phase 1 | Study EC2, S3, IAM, AWS Budget; define the digital-product marketplace topic. | Identified the required AWS services and the MVP scope.|
|Phase 2 | Initialize React/Vite + Node.js/Express + Prisma/PostgreSQL; build the database schema. | Foundation for auth, product, category, order.|
|Phase 3 | Develop the frontend, 3D viewer, admin dashboard, search, product detail, SePay flow. | Core business features completed.|
|Phase 4 | Deploy the backend to EC2, connect RDS, fix Prisma/RDS SSL issues, seed required data. | Public API working, cloud database running stably.|
|Phase 5 | Deploy the frontend to Vercel, fix SPA routing/API rewrites, migrate product storage to S3. | MVP deployed successfully: Vercel + EC2 + RDS + S3 + IAM Role.|
|Next phase | Complete CloudFront/Route 53/ACM, CloudWatch monitoring, CI/CD, and presigned URLs. | Closer to the production architecture.|


### 6. Budget Estimation
The infrastructure is designed for a low-cost learning/demo environment. The final numbers will be updated using AWS Billing or the AWS Pricing Calculator at submission time, because pricing depends on the region, instance type, runtime, storage volume, and data transfer.

<!--
{{% notice warning %}}
**Figure 2.2 not available yet** — Screenshot of AWS Billing/Budget or the AWS Pricing Calculator showing the estimated monthly cost for EC2, RDS, S3, and data transfer. Purpose: serves as evidence of the actual budget figures instead of theoretical estimates.
{{% /notice %}}
-->

Service | Configuration/Usage scope | Status | Estimate/notes to update
|---|---|---|---|
|Amazon EC2 | 1 Ubuntu instance running the Node.js backend with PM2 | In use | ~7.59$/month |
|Amazon RDS PostgreSQL | Single-AZ, db.t4g.micro, 20 GiB gp3 | In use | > ~12$/month |
|Amazon S3 | Bucket `marketplace-frontend-thao`, prefix `products/` for product assets | In use | ~0.18$/month |
|IAM Role/Policy | `marketplace-ec2-s3-role` | In use | No separate charge.|
|Vercel | frontend hosting | In use | Free |
|AWS Budget | Cost/log monitoring | Partial | AWS Budget has no base charge|


### 7. Risk Assessment
Risk | Impact | Probability | Mitigation strategy
|---|---|---|---|
|CloudFront/Route 53 unavailable due to AWS account verification | Medium | Medium | Use Vercel as temporary frontend hosting; keep CloudFront as the later production option.|
|Prisma/RDS connection error due to SSL | High | Already occurred | Configure SSL in the Prisma adapter; test with the pg driver and the Prisma runtime before running the seed.|
|DB metadata out of sync with local files on EC2 | High | Already occurred | Migrate product storage to S3; avoid depending on `backend/storage/products`.|
|Wrong S3 permissions causing upload/download failures | High | Medium | Use an IAM Role for EC2; limit the policy to the `products/` prefix and test Put/Get/Delete.|
|Deleting a product that has orders causes foreign key errors | Medium | Medium | Do not hard-delete products with orders; propose soft delete/isActive in a later phase.|
|Payment/webhook status mismatch | High | Medium | Use the DAIM order code + idempotency; check the order status and log webhooks.|
|Cloud cost overrun during the demo phase | Medium | Low-Medium | Enable budget alerts, use Single-AZ, turn off unneeded services, avoid NAT/ALB/WAF until required.|


### 8. Expected Outcomes
- Complete a digital-product marketplace MVP including login/register, search, product detail, 3D preview, admin product/category management, orders, and library.
- The backend runs stably on Amazon EC2 and connects to Amazon RDS PostgreSQL for relational data.
- Product files, thumbnails, and preview models are stored on Amazon S3 instead of the EC2 local disk.
- Users can only view/download files after the backend verifies the JWT and product ownership.
- The SePay webhook automatically updates the payment status from PENDING to SUCCESS.
- The project has a clear architecture diagram covering compute, storage, database, IAM, and payment integration.
- Forms the basis for expanding to seller approval, soft-delete products, CloudFront/Route 53/ACM, CloudWatch logging, CI/CD, and presigned URLs.

<!--
### 9. List of figures/evidence to be added
{{% notice info %}}
The figures below are **not available yet** and will be inserted into the report once captured/drawn. The table describes exactly what each figure must show.
{{% /notice %}}

Figure | Name/evidence | Content to show
|---|---|---|
|Figure 2.1 | DaiMarket AWS architecture diagram | Vercel or CloudFront, EC2 backend, RDS, S3 product assets, IAM Role, SePay webhook.|
|Figure 2.2 | AWS Billing/Budget or Pricing Calculator | Estimated monthly cost for EC2, RDS, S3, and data transfer.|
|Figure 2.3 | EC2 IAM Role | Backend instance with the IAM Role `marketplace-ec2-s3-role` attached.|
|Figure 2.4 | S3 bucket `products/` | New objects appearing after the admin uploads a product.|
|Figure 2.5 | API health/products/categories | curl or browser returning a successful response.|
|Figure 2.6 | Frontend demo | Home/search/product detail/library/admin pages working on Vercel.|
|Figure 2.7 | SePay webhook/order status | Webhook receiving the request and the order switching to SUCCESS with a valid test transaction.|
-->